# Sorting

## Sorting

> **Selection Sort:** Unstable \(Can be turned stable\), Ω\(N²\) - θ\(N²\) - O\(N²\) : O\(1\)  
> **Bubble Sort:** Stable**,** Ω\(N\) - θ\(N²\) - O\(N²\) : O\(1\)  
> **Insertion Sort:** Stable, Ω\(N\) - θ\(N²\) - O\(N²\) : O\(1\)  
> **Merge Sort:** Stable, Ω\(NlogN\) - θ\(NlogN\) - O\(NlogN\) : O\(N\)  
> **Quick Sort:** Unstable \(Can be turned stable\), Ω\(NlogN\) - θ\(NlogN\) - O\(N²\) : O\(1\)

**Insertion sort** is **faster** for **small** n because Quick **Sort** has extra overhead from the recursive function calls. Due to recursion constant factor is higher. **Insertion sort** requires less memory then quick sort \(stack memory\).

```cpp
void insertionSort(vector<int> &arr, int n)
{
    for (int i = 1; i < n; ++i)
    {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key)
            arr[j + 1] = arr[j], j--;
        arr[j + 1] = key;
    }
}

// In-place merge sort makes time complexity O(N²)
void inplaceMerge(vector<int> &arr, int l, int m, int r)
{
    for (int i = r; i > m; --i)
    {
        if (arr[m] > arr[i])
        {
            swap(arr[m], arr[i]);
            int tmp = arr[m];
            int j = m-1;
            while (j >= 0 && tmp < arr[j])
                arr[j+1] = arr[j], j--;
            arr[j+1] = tmp;
        }
    }
}
void merge(vector<int> &arr, int l, int m, int r)
{
    vector<int> temp = arr;
    int i = l, j = m + 1, k = l;
    while (i <= m && j <= r)
    {
        if (temp[i] > temp[j])
            arr[k++] = temp[j++];
        else
            arr[k++] = temp[i++];
    }
    while (i <= m)
        arr[k++] = temp[i++];
    while (j <= r)
        arr[k++] = temp[j++];
}
void mergeSort(vector<int> &arr, int l, int r)
{
    if (l < r)
    {
        m = (l + r) / 2;
        mergeSort(arr, 0, m);
        mergeSort(arr, m + 1, r);
        merge(arr, 0, m, r);
    }
}
/* Randomized quick sort (picking random pivot index instead of r) gives
NlogN in worst aswell */
// Quick sort is worst when - already sorted (asc or desc) or all elems are same
int partition(vector<int> &arr, int l, int r)
{
    int pivot = arr[r];
    int j = l;
    for (int i = l; i < r; ++i)
    {
        if (arr[i] < pivot)
            swap(arr[j++], arr[i]);
    }
    swap(arr[r], arr[j]);
    return j;
}
void sort(vector<int> &arr, int l, int r)
{
    if (l < r)
    {
        int partitionIndex = partition(arr, l, r);
        sort(arr, l, partitionIndex - 1);
        sort(arr, partitionIndex + 1, r);
    }
}
stable_partition(vec.begin(), vec.end());
```

> **Heap Sort:** Unstable, Ω\(NlogN\) - θ\(NlogN\) - O\(NlogN\) : O\(1\)  
> **Counting Sort:** Unstable, O\(N\) : O\(A\[i\]\)

```cpp
/* Heap is in array form, for all leaves i.e. from i = 0 to n/2
in reverse order heapify is called, child then root otherwise
child will cause unnecessary changes. After getting proper heap,
it's 0th element gives max (or min in min heap) take that and
put it at the end now ignore thatlast value because it's sorted
and again apply heapify on theremaining.*/
void heapify(int arr[], int n, int i)    // max heap here
{
    int parent = i, left = 2 * i + 1, right = 2 * i + 2;
    if (left < n && arr[parent] < arr[left])
    {
        swap(arr[left], arr[parent]);
        heapify(arr, n, left);
    }
    else if (right < n && arr[parent] < arr[right])
    {
        swap(arr[right], arr[parent]);
        heapify(arr, n, right);
    }
}
void heapSort(int arr[], int n)
{
    // Building heap part is O(N)
    // https://www.geeksforgeeks.org/time-complexity-of-building-a-heap/
    for (int i = n / 2 - 1; i >= 0; --i)
        heapify(arr, n, i);
    // ascending order sort
    for (int i = n - 1; i >= 0; --i)
    {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

### Radix Sort

[https://www.youtube.com/watch?v=JMlYkE8hGJM](https://www.youtube.com/watch?v=JMlYkE8hGJM)

O\(d \* \(n + b\)\) - b is base in number it is 10, for strings it's 26

* Most optimal, only drawback is due to not being comparison based sorting in nature only limited to int, float or strings.

```cpp
void radixSort(vector<int> &arr)
{
    int d = log10(*max_element(arr.begin(), arr.end())) + 1;
    vector<int> bucket[10];
    /* Move from LSB to MSB (digit), get that dig from each
    element in arr and put it on buckets of 0-9.
    In the end iterate over buckets and preserve that order */ 
    for (int i = 0, pos = 10; i < d; ++i, pos *= 10)
    {
        for (auto &x : bucket) x.clear();
        for (auto &x : arr)
        {
            int val = ((x % pos) / (pos/10));
            bucket[val].push_back(x);
        }
        arr.clear();
        for (auto &x : bucket)
            for (auto &y : x) arr.push_back(y);
    }
}

// optimized space usage implementation
void radixSort(vector<int> &arr)
{
    vector<int> bucket(10), aux(arr.size());
    int d = log10(*max_element(arr.begin(), arr.end())) + 1;
    for (int i = 0, pos = 10; i < d; ++i, pos *= 10)
    {
        fill(bucket.begin(), bucket.end(), 0);
        for (int i = 0; i < arr.size(); ++i) bucket[(arr[i] % pos) / (pos/10)]++;
        for (int i = 1; i < 10; ++i) bucket[i] += bucket[i-1];
        for (int i = arr.size()-1; i >= 0; --i)
        {
            int x = (arr[i] % pos) / (pos/10);
            aux[--bucket[x]] = arr[i];
        }
        for (int i = 0; i < arr.size(); ++i) arr[i] = aux[i];
    }
}
```

## Partial Sorting

Time complexity: O\(n + klogk\)

```cpp
// 10, 45, 60, 78, 23, 21, 3
partial_sort(vec.begin(), vec.begin() + 3, vec.end());
// gives 3 10 21 78 60 45 23
/* If say problem was add k elements from array in sorted order then doing it
with a priority queue is same as sorting all at once O(nlogn) using partial
sort is better */

// stl uses heap sort internally
```

### Kth Smallest/Largest Number

```cpp
// Quick Sort extension
using namespace std;
int partition(int arr[], int l, int r)
{
    int pivot = arr[r];
    int j = l - 1;
    for (int i = l; i <= r - 1; ++i)
        if (arr[i] <= pivot) swap(arr[++j], arr[i]);
    swap(arr[++j], arr[r]);
    return j;
}
int k_th_smallest(int arr[], int l, int r, int k)
{
    if (k > 0 && k <= r - l + 1)
    {
        int pos = partition(arr, l, r);
        if (pos - l == k - 1) return arr[pos];
        if (pos - l > k - 1) return k_th_smallest(arr, l, pos - 1, k);
        return k_th_smallest(arr, pos + 1, r, k - pos + l - 1);
    }
    return INT_MAX;
}
```

### Count Inversions

```cpp
// BruteFoce O(N²)
int invCount(vector<int> &arr)
{
    int inv_count = 0;
    for (int i = 0; i < arr.size(); ++i)
    {
        for (int j = i + 1; j < arr.size(); ++j)
            if (arr[i] > arr[j]) inv_count++;
    }
    return inv_count;
}

/* Merge Sort extension O(nlogn)
Whenever there's a swap while merging, there will be m-i+1 inversions */
int merge(vector<int> &arr, int l, int m, int r)
{
    vector<int> temp = arr;
    int i = l, j = m + 1, k = l, inv_count = 0;
    while (i <= m && j <= r)
    {
        if (temp[i] > temp[j]) arr[k++] = temp[j++], inv_count += m - i + 1;
        else arr[k++] = temp[i++];
    }
    while (i <= m) arr[k++] = temp[i++];
    while (j <= r) arr[k++] = temp[j++];
    return inv_count;
}
int mergeSort(vector<int> &arr, int l, int r)
{
    int inv_count = 0, m = (l + r) / 2;
    if (l < r)
    {
        inv_count += mergeSort(arr, 0, m);
        inv_count += mergeSort(arr, m + 1, r);
        inv_count += merge(arr, 0, m, r);
    }
    return inv_count;
}

/* Using Fenwick tree O(nlogn)
For an element x inside array we need to find how many elements are smaller
that it and appeared before. Initialize a BIT with all zero once element x
is visited update it's count +1 in BIT. Find query for element >= x
thats inv count */
int invCount(vector<int> &arr)
{
    int inv_count = 0;
    for (auto &x : arr)
    {
        inv_count += query(MAXN) - query(x);
        update(x, 1);
    }
}
```

### Minimum Number of Swaps required to Sort array

```cpp
/* Way #1 - Use selection Sort if there's a swap, count it
O(N^2) however it can be optimized through heap giving
O(NlogN) */
int solve(int &n, vector<int> &arr)
{
    int count = 0;
    priority_queue< pair<int, int>, vector< pair<int, int> >, greater< pair<int, int> > > pq;
    for (int i = 1; i < n; ++i) pq.push({arr[i], i});
    for (int i = 0; i < n-1; ++i)
    {
        int idx = pq.top().second;
        pq.pop();
        if (arr[idx] != arr[i])
        {
            int temp = arr[idx];
            arr[idx] = arr[i];
            arr[i] = temp;
            count++;
        }
    }
    return count;
}
/* But the code only works for distinct elements
 5 1 2 2 2 2 2 => should give 2 but gives 6
 We can do one thing instead of having idx to the leftmost
 we should get the rightmost one */
int solve(int &n, vector<int> &arr)
{
    int count = 0;
    unordered_map<int, int> keyToIndex;
    for (int i = 0; i < n; ++i) keyToIndex[arr[i]] = i;
    priority_queue<int, vector<int>, greater<int> > pq;
    for (int i : arr) pq.push(i);
    for (int i = 0; i < n-1; ++i)
    {
        int smallest = pq.top();
        pq.pop();
        if (smallest != arr[i])
        {
            int temp1 = arr[i];
            arr[i] = arr[keyToIndex[smallest]];
            arr[keyToIndex[smallest]] = temp1;

            int temp2 = keyToIndex[smallest];
            keyToIndex[smallest] = keyToIndex[temp1];
            keyToIndex[temp1] = temp2;
            count++;
        }
    }
    return count;
}
/*
The idea is that if a occupies b's position, b occupies c's
position and so on, then there will be some integer x which
will occupy a's position. So, this forms a cycle.
So, if any element arr[i] is not at its correct position, we
shift it to its correct position j, then shift arr[j] to its
correct position k and so on. So, if len is the length of the
cycle (number of elements in the cycle), then it will require
a minimum of len-1 swaps to rearrange the elements of the cycle
to their correct positions.
We find all such cycles and compute our answer.
*/
int DFS(vector<int> adj[], vector<bool> &visited, int cur)
{
    visited[cur] = true;
    int val = 1;
    for (auto x : adj[cur])
    {
        if (!visited[x])
            val += DFS(adj, visited, x);
    }
    return val;
}
int solve(int &n, vector<int> &arr)
{
    vector<int> adj[n];
    vector<bool> visited(n, false);
    for (int i = 0; i < n; ++i) adj[i].push_back(arr[i] - 1), adj[arr[i]-1].push_back(i);
    int count = 0;
    for (int i = 0; i < n; ++i)
    {
        if (!visited[i])
            count += DFS(adj, visited, i) - 1;
    }
    return count;
}
```

### [Merge Intervals](https://leetcode.com/problems/merge-intervals)

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals)
    {
        vector<vector<int>> res;
        if (intervals.size() == 0)
            return res;
        sort(intervals.begin(), intervals.end(), [](auto &x, auto &y)
        {
            return (x[0] == y[0]) ? (x[1] < y[1]) : (x[0] < y[0]);
        });
        res.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); ++i)
        {
            if (intervals[i][0] <= res.back()[1])
                res[res.size()-1][1] = max(res[res.size()-1][1], intervals[i][1]);
            else
                res.push_back(intervals[i]);
        }
        return res;
    }
};

// O(1) space solution, N^2 time
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals)
    {
        if (intervals.size() == 0) return intervals;
        sort(intervals.begin(), intervals.end(), [](auto &x, auto &y)
        {
            return (x[0] == y[0]) ? (x[1] < y[1]) : (x[0] < y[0]);
        });

        vector<vector<int>>::iterator back = intervals.begin();
        for (auto it = ++intervals.begin(); it != intervals.end();)
        {
            if ((*it)[0] <= (*back)[1])
            {
                (*back)[1] = max((*back)[1], (*it)[1]);
                intervals.erase(it);    // this increases complexity
            }
            else back = it++;
        }
        return intervals;
    }
};
```

### [Insert Interval In a Sorted List](https://leetcode.com/problems/insert-interval/)

In 2 pass found start and end bound. Start is max index with intervals\[start\]\[1\] &lt;= newInterval\[0\] and end is min index with intervals\[end\]\[0\] &lt;= newInterval\[1\]

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval)
    {
        if (intervals.empty()) return {newInterval};
        int l = 0, r = intervals.size()-1;
        while (l <= r)
        {
            int mid = (l+r)/2;
            if (intervals[mid][1] == newInterval[0]) { l = mid; break; }
            if (intervals[mid][1] < newInterval[0]) l = mid+1;
            else r = mid-1;
        }
        int start = l; r = intervals.size()-1;
        while (l <= r)
        {
            int mid = (l+r)/2;
            if (intervals[mid][0] == newInterval[1]) { r = mid; break; }
            if (intervals[mid][0] < newInterval[1]) l = mid+1;
            else r = mid-1;
        }
        int end = r;
        if (start < intervals.size())
            newInterval[0] = min(newInterval[0], intervals[start][0]);
        if (end >= 0)
            newInterval[1] = max(newInterval[1], intervals[end][1]);
        intervals.erase(intervals.begin()+start, intervals.begin()+end+1);
        intervals.insert(intervals.begin()+start, newInterval);
        return intervals;
    }
};
```

### [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)

```cpp
vector<vector<int>> intervalIntersection(vector<vector<int>>& A, vector<vector<int>>& B)
{
    vector<vector<int>> res;
    for (int i = 0, j = 0; i < A.size() && j < B.size();)
    {
        int p = max(A[i][0], B[j][0]);
        int q = min(A[i][1], B[j][1]);
        if (p <= q)
        {
            res.push_back({p, q});
            if (q == A[i][1]) ++i;
            else ++j;
        }
        else
        {
            if (A[i][0] > B[j][1]) ++j;
            else ++i;
        }
    }
    return res;
}
```

### [Hotel Booking Problem](https://www.interviewbit.com/problems/hotel-bookings-possible/)

```cpp
/* Consider it like a timeline +1 means arrival -1 means departure then sort it
and add to find rooms required at an instance. */
bool Solution::hotel(vector<int> &arrive, vector<int> &depart, int K)
{
    vector<pair<int, int>> v;
    int n = arrive.size();
    for (int i = 0; i < n; ++i)
    {
        v.push_back({arrive[i], 1});
        v.push_back({depart[i], -1});
    }
    sort(v.begin(), v.end());
    int count = 0;
    for (auto &x : v)
    {
        count += x.second;
        if (count > k) return false;
    }
    return true;
}
```

### [Largest Number](https://www.interviewbit.com/problems/largest-number/)

```cpp
string Solution::largestNumber(const vector<int> &A)
{
    vector<string> b;
    for (auto &x : A) b.push_back(to_string(x));
    sort(b.begin(), b.end(), [](string x, string y)
    {
        string xy = x.append(y);
        string yx = y.append(x);
        return xy.compare(yx) > 0 ? true : false;
    });
    string ans = "";
    for (int i = 0; i < b.size(); i++) ans += b[i];
    bool allzero = true;
    for (char i : ans)
        if (i != '0') { allzero = false; break; }
    if (allzero) return "0";
    return ans;
}
```

### [Max Distance](https://www.interviewbit.com/problems/max-distance/)

```cpp
int Solution::maximumGap(const vector<int> &A)
{
    int n = A.size();
    if (n == 0) return -1;
    vector<pair<int, int>> arr;
    for (int i = 0; i < n; ++i) arr.push_back({A[i], i});
    sort(arr.begin(), arr.end());
    int ans = 0, rmax = arr[n - 1].second;
    for (int i = n - 2; i >= 0; --i)
    {
        ans = max(ans, rmax - arr[i].second);
        rmax = max(rmax, arr[i].second);
    }
    return ans;
}
```

### Missing Integer in an array

```cpp
/* If the array contains all numbers from 1 to N except one of them is missing
(n*(n+1))/2 - sum_of_array is answer */

/* First missing integer
Mark every element visited as negative within array only */
for (auto it = A.begin(); it != A.end();) // first remove negative numbers
{
    if (*it <= 0) A.erase(it);
    else ++it;
}
for (auto &x : A)
{
    int cur = abs(x)-1;
    if (cur >= 0 && cur < A.size()) A[cur] = -A[cur];
}
for (int i = 0; i < A.size(); i++)
    if (A[i] > 0) return i + 1;
return A.size()+1;
```

### [Missing Ranges](https://www.lintcode.com/problem/missing-ranges/description)
```c++
vector<string> findMissingRanges(vector<int> &nums, int lower, int upper)
{
    vector<string> res;
    long long l = lower, r = upper;
    for (const int x : nums)
    {
        if (x > l)
        {
            if (l == x-1) res.push_back(to_string(l));
            else res.push_back(to_string(l) + "->" + to_string(x-1));
        }
        l = (long long)x + 1;
    }
    if (l <= r)
    {
        if (l == r) res.push_back(to_string(l));
        else res.push_back(to_string(l) + "->" + to_string(r));
    }
    return res;
}
```

### Maximum Consecutive Gap

```cpp
/* Basically if we could have sort then it's simple how about counting sort?
It would have worked if number range was low.
We have to reduce comparisons in sorting, we can use idea of bucket sort.
Considering uniform gap:
min, min + gap, min + 2*gap, ... min + (n-1)*gap
min + (n-1)*gap = max
gap = (max - min) / (n-1)
Now we place every number in these n-1 buckets:
bucket = (num - min)/gap, we prepare max and min for each bucket and subtract
to find our answer */
int Solution::maximumGap(const vector<int> &A)
{
    int n = A.size();
    if (n < 2) return 0;
    vector<int> forMin(n, INT_MAX), forMax(n, INT_MIN);
    int mn = *min_element(A.begin(), A.end());
    int mx = *max_element(A.begin(), A.end());
    int gap = (mx-mn) / (n-1);
    if (gap == 0) return (mx-mn);
    for (auto &x : A)
    {
        int bucket = (x - mn) / gap;
        forMin[bucket] = min(x, forMin[bucket]);
        forMax[bucket] = max(x, forMax[bucket]);
    }
    int maxDiff = 0;
    for (int i = 0, j = 0; i < forMin.size(); ++i)
    {
        if (forMin[i] >= 0)
        {
            maxDiff = max(maxDiff, forMin[i] - forMax[j]);
            j = i;
        }
    }
    return maxDiff;
}
```

### Min/Max XOR Pair

```cpp
/* If we sort and then check xor of consecutive pair it will give min
no need to check pair which are not consecutive. */
int Solution::findMinXor(vector<int> &A)
{
    sort(A.begin(), A.end());
    int mn = INT_MAX;
    for (int i = 0; i < A.size()-1; ++i)
        mn = min(ans, A[i]^A[i+1]);
    return mn;
}
// Only work for min XOR
```

Can be solved using trie along with Max XOR Pair problem

```cpp
/* No need to check for isTerminal since we have same length
(i.e. 32) for all */
vector<vector<int>> trieArr;
int nxt;
void insert(int num)
{
    int pos = 0;
    for (int i = 31; i >= 0; --i)
    {
        bool cur = !!(num & (1<<i));
        if (trieArr[pos][cur] == 0)
        {
            trieArr[pos][cur] = nxt;
            pos = nxt++;
        }
        else pos = trieArr[pos][cur];
    }
}
int findOptimal(int num)
{
    int pos = 0, res = 0;
    for (int i = 31; i >= 0; --i)
    {
        bool cur = !!(num & (1<<i));
        if (trieArr[pos][!cur] != 0)
        {
            pos = trieArr[pos][!cur];
            if (!cur) res += (1<<i);
        }
        else
        {
            pos = trieArr[pos][cur];
            if (cur) res += (1<<i);
        }
    }
    return res;
}
int findMaximumXOR(vector<int>& nums)
{
    trieArr.assign((nums.size()+1)*32, vector<int>(2, 0));
    nxt = 1;
    
    for (auto &x : nums) insert(x);
    int res = 0;
    for (auto &x : nums) res = max(res, x^findOptimal(x));
    return res;
}
```

### [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

![\[ \[2 9 10\], \[3 7 15\], \[5 12 12\], \[15 20 10\], \[19 24 8\] \] -&amp;gt; \[ \[2 10\], \[3 15\]...](.gitbook/assets/image%20%28264%29.png)

Can also be solved using segment tree

```cpp
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings)
    {
        vector<pair<int, int>> edges;
        for (auto &x : buildings)
        {
            edges.push_back({x[1], x[2]});
            edges.push_back({x[0], -x[2]});
        }
        sort(edges.begin(), edges.end());
        
        /*
            [9, 10]                    [2, -10]
            [2, -10]                   [3, -15]
            [7, 15]                    [5, -12]
            [3, -15]                   [7, 15]
            [12, 12]        ->         [9, 10]
            [5, -12]                   [12, 12]
            [20, 10]                   [15, -10]
            [15, -10]                  [19, -8]
            [24, 8]                    [20, 10]
            [19, -8]                   [24, 8]
        */

        multiset<int> st;
        st.insert(0);
        int cur = 0;
        vector<vector<int>> res;
        for (auto &x : edges)
        {
            if (x.second < 0) st.insert(-x.second); // -ve is starting
            else st.erase(st.find(x.second));
            if (cur != *st.rbegin())
            {
                res.push_back({x.first, *st.rbegin()});
                cur = *st.rbegin();
            }
        }
        return res;
    }
};
```

