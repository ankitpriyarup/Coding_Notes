# Searching

## Complete Search

The idea is to generate all possible solutions to the problem using brute force, and then select the best solution or count the number of solutions, depending on the problem.

```cpp
// Generating subsets
void search(int k)
{
    if (k == n) // process subset
    else
    {
        search(k+1);
        subset.push_back(k);
        search(k+1);
        subset.pop_back();
    }
}

// Generating subsets - Iterative (using bitmanipulation)
for (int i = 0; i < (1<<n); ++i)
{
    int x = i, pos = 0;
    while (x)
    {
        if (x&1) take(pos);
        pos++;
        x <<= 1;
    }
    // process permutation
}

// Generating Permutations
void search()
{
    if (permutation.size() == n) // Process permutation
    else
    {
        for (int i = 0; i < n; ++i)
        {
            if (chosen[i]) continue;
            chosen[i] = true;
            permutation.push_back(i);
            search();
            chosen[i] = false;
            permutation.pop_back();
        }
    }
}

// Generating Permutations Iterative (using next_permutation)
vector<int> permutation(n);
for (int i = 0; i < n; ++i) permutation[i] = i+1;
do
{
    // Process permutation
} while(next_permutation(permutation.begin(), permutation.end()));
```

## Backtracking

Smart Complete Search

```cpp
// N queens problem
void search(int y)
{
    if (y == n) { found++; return; }
    for (int x = 0; x < n; ++x)
    {
        if (col[x] || diag1[x+y] || diag2[x-y+n-1]) continue;
        col[x] = diag1[x+y] = diag2[x-y+n-1] = true;
        search(y+1);
        col[x] = diag1[x+y] = diag2[x-y+n-1] = false;
    }
}
/* Let q(n) be ans for n.
There is no known way to efficiently calculate larger values of q(n).
The current record is q(27) = 234907967154122528 */
```

## Pruning the Search

We can often optimize backtracking by pruning the search tree. The idea is to add ”intelligence” to the algorithm so that it will notice as soon as possible if a partial solution cannot be extended to a complete solution.

Let us consider the problem of calculating the number of paths in an n x n grid from upper-left corner to lower-right corner such that the path visits each square exactly once.

![For 7x7 there are 111712 such paths](.gitbook/assets/image%20%2839%29.png)

* Optimization 1 \(Simple backtracking\) **Running time 783 seconds, 76 billion Recursive calls**
* Optimization 2 \(If path reaches lowest right square before it has visited all other squares of the grid, it is clear that it will not be possible to complete the solution. **Running time 119 seconds, 20 billion Recursive calls**

![Terminate search immediately if we reach lower-right square too early](.gitbook/assets/image%20%2823%29.png)

* Optimization 3 \(If path touches the wall it can either go left or right in that split two-part case one part will get unvisited\) **Running time 1.8 seconds, 221 million Recursive calls**

![](.gitbook/assets/image%20%2882%29.png)

* Optimization 4 \(Idea of previous optimization can be genralized: if path cannot continue forward but can turn either left or right, the grid splits into two pat that both contain unvisited squares\) **Running time 0.6 seconds, 69 million Recursive calls**

> Optimzation made it over 1000 times faster now

## Meet in the middle

Meet in the middle is a technique where the search space is divided into two parts of about equal size. A separate search is performed for both of the parts, and finally the results of the searches are combined.

For example, suppose that the list is \[2,4,5,9\] and x = 15. First, we divide the list into A = \[2,4\] and B = \[5,9\]. After this, we create lists SA = \[0,2,4,6\] and SB = \[0,5,9,14\]. \(SA is total subset sum possible in A\) Next we can find original problem solution by using hashmap SA\[i\] + SA\[j\] = targ.

This makes the complexity O\(2^\(n/2\)\) which is also O\(root 2^n\)

## Binary Search

```cpp
// Simple style
int l = 0, r = n-1;
while (l <= r)
{
    int mid = l + (r-l)/2;
    if (arr[mid] == x) return true;
    if (arr[mid] > x) r = mid-1;
    else l = mid+1;
}
return false;

// Constraint-bound style
int l = 0, r = n-1;
while (l < r)
{
    int mid = l + (r-l)/2;
    if (check(mid)) l = mid+1;
    else r = mid;
}
return l;

/* Jump-based style
Idea is to make jumps and slow the speed when we get closer to the target
element. Search goes from left to right, and initial jump length is n/2.
At each step, the jump length is halved. After the jump either the target
element has been found or we know that it does not appear in the array. */
int j = 0;
for (int i = n/2; i >= 1; i /= 2)
{
    while (i+j < n && arr[i+j] <= x)    // run atmost twice
        j += i;
}
return (arr[j] == x);
```

### Unbounded Binary Search

Consider that there's a monotonically increasing function f\(x\) with f\(0\) some negative value we need to find some value n for which f\(n\) will be the first non negative number of the function.  
Naive approach is linearly searching till we get number greater than 0, it will take O\(n\)  
Other approach is using Unbounded Binary Search. The idea is to proceed with f\(0\) then f\(1\) f\(2\) f\(4\) f\(8\) f\(16\) ... till f\(x\) every other is ''''''''''''''''''''''x2 of previous. Now f\(x\) is first non negative in the above exponentiated series. Now we need to apply binary search from O\(x/2\) to O\(x\) x/2 is previous term of the series. This will give a time complexity of O\(logn\)

### STL Implementations

Following STL functions revolve around binary search \(assumption array is sorted\):

* lower\_bound: first array element whose value is at least x
* upper\_bound: first array element whose value is larger than x
* equal\_range: returns both above pointers

```cpp
auto a = lower_bound(array, array+n, x);
auto b = upper_bound(array, array+n, x);
cout << b-a << "\n";

auto r = equal_range(array, array+n, x);
cout << r.second-r.first << "\n";
// Both are equivalent
```

## [Codeforces Edu - Binary Search](https://codeforces.com/edu/course/2/lesson/6)

### [Step 1 A - Binary Search](https://codeforces.com/edu/course/2/lesson/6/1/practice/contest/283911/problem/A)

```cpp
int n, k; cin >> n >> k;
vector<int> arr(n);
for (auto &x : arr) cin >> x;
while (k--)
{
    int x; cin >> x;
    int l = 0, r = n-1;
    bool found = false;
    while (l <= r)
    {
        int mid = l + (r-l)/2;
        if (arr[mid] == x) { found = true; break; }
        if (arr[mid] > x) r = mid-1;
        else l = mid+1;
    }
    if (found) cout << "YES\n";
    else cout << "NO\n";
}
```

### [Step 1 B - Closest to the Left](https://codeforces.com/edu/course/2/lesson/6/1/practice/contest/283911/problem/B)

```cpp
int n, k; cin >> n >> k;
vector<int> arr(n);
for (auto &x : arr) cin >> x;
while (k--)    // k queries of how many numbers > x in arr
{
    int x; cin >> x;
    int l = 0, r = n, res = 0;
    while (l < r)
    {
        int mid = l + (r-l)/2;
        if (arr[mid] <= x) l = mid+1, res = mid+1;
        else r = mid;
    }
    cout << res << '\n';
}
```

### [Step 1 C - Closest to the Right](https://codeforces.com/edu/course/2/lesson/6/1/practice/contest/283911/problem/C)

```cpp
int n, k; cin >> n >> k;
vector<int> arr(n);
for (auto &x : arr) cin >> x;
while (k--)    // k queries of how many numbers atleast x in arr
{
    int x; cin >> x;
    int l = 0, r = n, res = n+1;
    while (l < r)
    {
        int mid = l + (r-l)/2;
        if (arr[mid] >= x) r = mid, res = mid+1;
        else l = mid+1;
    }
    cout << res << '\n';
}
```

### [Step 1 D - Fast search](https://codeforces.com/edu/course/2/lesson/6/1/practice/contest/283911/problem/D)

```cpp
int n; cin >> n;
vector<int> arr(n);
for (auto &x : arr) cin >> x;
sort(arr.begin(), arr.end());

auto cnt = [&](int x)
{
    int l = 0, r = n, res = 0;
    while (l < r)
    {
        int mid = l + (r-l)/2;
        if (arr[mid] > x) r = mid;
        else l = mid+1, res = mid+1;
    }
    return res;
};

int k; cin >> k;    // Given k queries find count of numbers b/w [L,R] in arr
while (k--)
{
    int L, R; cin >> L >> R;
    cout << (cnt(R) - cnt(L-1)) << ' ';
}
cout << '\n';
```

### [Step 2 A - Packing Rectangles](https://codeforces.com/edu/course/2/lesson/6/2/practice/contest/283932/problem/A)

```cpp
int w, h, n; cin >> w >> h >> n;
auto check = [&](int x) { return ((x/w) * (x/h) >= n); };

int l = 0, r = 1;
while (!check(r)) r *= 2;
while (l+1 < r)
{
    int mid = l + (r-l)/2;
    if (!check(mid)) l = mid;
    else r = mid;
}
cout << r << '\n';
```

### [Step 2 B - Ropes](https://codeforces.com/edu/course/2/lesson/6/2/practice/contest/283932/problem/B)

```cpp
const double EPS = 1e-6;
int n, k; cin >> n >> k;
vector<int> arr(n);
for (auto &x : arr) cin >> x;
double l = 0, r = 1e9;
while (l + EPS < r)
{
    double mid = (l + r)/2;
    int cnt = 0;
    for (const int x : arr) cnt += x/mid;
    if (cnt < k) r = mid;
    else l = mid;
}
cout << setprecision(6) << fixed << l << '\n';
```

### [Step 2 C - Very Easy Task](https://codeforces.com/edu/course/2/lesson/6/2/practice/contest/283932/problem/C)

```cpp
int n, x, y; cin >> n >> x >> y;
auto check = [&](int v)
{
    v -= min(x, y);
    if (v < 0) return (n == 0);
    int copies = (n-1) - max(0LL, v/x) - max(0LL, v/y);
    return (copies <= 0);
};
int l = 0, r = 1;
while (!check(r)) r *= 2;
while (l+1 < r)
{
    int mid = (l + r)/2;
    if (check(mid)) r = mid;
    else l = mid;
}
cout << r << '\n';
```

## Hashing

> Searching takes O\(1\), Terminologies - Hashing, Hashfunction, Hashtable

Hashfunction requirements - Uniformly distributed, Easy to compute, minimize collision.

* **Division Method:** h\(k\) = k mod n where n is size of hashtable.
* **Folding Method:** divide in group of 2 and add like 123456 = 12+34+56 = 102 ignore 1 map it to 02
* **Mid Square Method:** 125 square is 15625 taking mid 56 \(If non even digits then append zero at begining\)
* **MAD \(Multiplication Addition & Division\):** h\(k\) = \(a.k + b\) % n
* **Multiplication Method:** \(kA % 1\)\*n where A is golden ratio constant 0.618, 0 &lt; A &lt; 1

### Collision Resolution

![](.gitbook/assets/image%20%28218%29.png)

Closed Hashing \(Open Addressing\) - We don't utilize any thing extra then hash table everything is still mapped in it unlike linked list like ADT

Open Hashing

* Chaining: Using linked list like chain to store collisions.

Closed Hashing H\(i\) = \(h\(k\) + f\(i\)\) % n

* Linear Probing: f\(n\) = i
* Quadratic Probing: f\(n\) = i^2

We go for H\(0\) first if collision H\(1\) and so on.

![Linear Probing](.gitbook/assets/image%20%28263%29.png)

The main problem with linear probing is clustering, many consecutive elements form groups and it starts taking time to find a free slot or to search an element.

> Modulo n where n is there in good hash functions so that collision reduces.

## Solve Monotonically increasing functions

[https://www.youtube.com/watch?v=Y2AUhxoQ-OQ](https://www.youtube.com/watch?v=Y2AUhxoQ-OQ)

Given p, q, r, s, t, u we want to find x  
p_e^-x + q_sin\(x\) + r_cos\(x\) + s_tan\(x\) + t\*x2 + u = 0

```cpp
#include<bits/stdc++.h>
using namespace std;

int p, q, r, s, t, u;
#define EPS 1e-7
double f(double x) { return p*exp(-x) + q*sin(x) + r*cos(x) + s*tan(x) + t*x*x + u; }

// Bisection method
double bisection()
{
    double l = 0, r = 1;
    while (l + EPS < r)
    {
        double mid = (l + r)/2;
        if (f(l) * f(mid) <= 0) r = mid;
        else l = mid;
    }
    return (l + r)/2;
}
int main()
{
    while (cin >> p >> q >> r >> s >> t >> u)
    {
        // Here f is a non-increasing function [0,1]
        // So if signs are same then no solution
        if (f(0) * f(1) > 0) cout << "No solution\n";
        else cout << fixed << setprecision(4) << bisection() << '\n';
    }
    return 0;
}

// More efficient
double secant()
{
    if (f(0) == 0) return 0;
    for (double x0 = 0, x1 = 1; ;)
    {
        double d = f(x1)*(x1-x0) / (f(x1)-f(x0));
        if (fabs(d) < EPS) return x1;
        x0 = x1;
        x1 = x1 - d;
    }
}

// Most efficient
double fd(double x) // derrivative of f(x)
{
    return -p*exp(-x) + q*cos(x) - r*sin(x) + s/(cos(x)*cos(x)) + 2*t*x;
}
double newton()
{
    if (f(0) == 0) return 0;
    for (double x = 0.5; ;)
    {
        double x1 = x - f(x)/fd(x);
        if (fabs(x1-x) < EPS) return x;
        x = x1;
    }
}
```

## Ternary Search

Ternary Search is also there which is same as binary search except instead of dividing in 2 parts we divide it in 3 parts.  
Binary search is better than ternary search because   
 T\(n\) = T\(n/2\) + 2 \[In Binary Search\]  
 T\(n\) = T\(n/3\) + 4 \[In Ternary Search\]  
 in binary search: $$2log_2n$$ complexity while in ternary search:$$4log_3n$$ calculating mathematically binary search is better.

> Ternary search can find value for a bell shaped curve \(unimodal function\)

## Additional Questions

### Duplicate Number problem

* Given array containing n+1 numbers of range: 1 to n \(inclusive\). There's 1 duplicate find it.

```cpp
/* bruteforce (n^2) -> sorting (nlogn) -> hashing (requires space)
To do in O(1) space we want to store hashing info to the array itself.
Given range 1 to n and there are n+1 numbers we can maybe make a number
negative.

We can also get summation sm - n(n+1)/2 */
for (const int x : nums)
{
    if (nums[abs(x)-1] < 0) return abs(x);
    nums[abs(x)-1] *= -1;
}

/* If we are not allowed to modify the array, cycle detection
extending upon previous idea only [1, 2, 3, 4, 2] 1 points to ind 1, 2 to ind 2
1 -> 2 -> 3 -> 4 -> 2
          |         |
          ----------
cycle exists means thats the duplicate, using floyd cycle detection algo */
int slow = nums[0], fast = nums[nums[0]];
while (slow != fast) { slow = nums[slow]; fast = nums[nums[fast]]; }
fast = 0;
while (slow != fast) { slow = nums[slow]; fast = nums[fast]; }
return slow;
```

* Now more than 1 elements can occur twice others once. Find all such duplicate numbers

```cpp
// Above idea only
vector<int> res;
for (const int x : nums)
{
    if (nums[abs(x)-1] < 0)
        res.push_back(abs(x));
    else
        nums[abs(x)-1] *= -1;
}
return res;

/* Extending above cycle detection idea. Instead of keeping visited
we are kinda caching it with cycling swaps. */
vector<int> res;
for (int i = 0; i < nums.size(); ++i)
{
    while (nums[i] != i+1 && nums[i] != nums[nums[i]-1])
        swap(nums[i], nums[nums[i]-1]);    // swaping makes code O(N) time
}
for (int i = 0; i < nums.size(); ++i)
    if (nums[i] != i+1) res.push_back(nums[i]);
return res;
```

* Find one duplicate in sorted array, with \[0, n\] numbers \(no number in between is missing\)

```cpp
int l = 0, r = n - 1;
while (l < r)
{
    int mid = l + (r - l) / 2;
    if (arr[mid] == mid + 1) l = mid+1;
    else
    {
        if (mid > 0 && arr[mid] == arr[mid-1]) return mid;
        r = mid-1;
    }
}
```

### [Peak Element](https://leetcode.com/problems/find-peak-element)

```cpp
int findPeakElement(vector<int> &nums)
{
    int l = 0, r = nums.size() - 1;
    while (l < r)
    {
        int mid = l + (r - l) / 2;
        if (nums[mid] > nums[mid + 1]) r = mid;
        else l = mid + 1;
    }
    return l;
}
```

### Square root of a number

```cpp
int Solution::sqrt(int A)
{
    if (A == 0) return 0;
    if (A == 1 || A == 2 || A == 3) return 1;
    int l = 0, r = A, ans = -1;
    while (l < r)
    {
        int mid = l + (r - l) / 2;
        if (mid <= A / mid) l = mid + 1, ans = mid;
        else r = mid;
    }
    return ans;
}
```

### [First and Last position of elements in sorted array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```cpp
class Solution
{
public:
    int lower_bound(vector<int> &nums, int target)
    {
        int l = 0, r = nums.size() - 1;
        if (nums[r] < target) return r + 1;
        while (l < r)
        {
            int mid = l + (r - l) / 2;
            if (nums[mid] < target) l = mid + 1;
            else r = mid;
        }
        return l;
    }
    vector<int> searchRange(vector<int> &nums, int target)
    {
        if (nums.size() == 0)
            return {-1, -1};
        int idx1 = lower_bound(nums, target);
        int idx2 = lower_bound(nums, target + 1) - 1;
        if (idx1 < nums.size() && nums[idx1] == target) return {idx1, idx2};
        else return {-1, -1};
    }
};
```

### Rotated Sorted Array Search

```cpp
/* If we look at middle, either all left side is sorted or right one is.
We can check that by checking first and last element of the partition */
int search(vector<int> &nums, int target)
{
    int l = 0, r = nums.size() - 1;
    while (l <= r)
    {
        int mid = l + (r - l) / 2;
        if (nums[mid] == target) return mid;
        // If there can be duplicates then add this cond.
        // Eg: [3 1 2 3 3 3 3]
        if (nums[l] == nums[mid] && nums[r] == nums[mid]) ++l, --r;
        else if (nums[l] <= nums[mid])
        {
            if (nums[l] <= target && nums[mid] >= target) r = mid - 1;
            else l = mid + 1;
        }
        else
        {
            if (nums[mid] <= target && nums[r] >= target) l = mid + 1;
            else r = mid - 1;
        }
    }
    return -1;
}
```

### [Exam Room](https://leetcode.com/problems/exam-room/)
```c++
// O(N) seat and O(logN) leave
int numOfStudents;
set<int> locations;
ExamRoom(int N) : numOfStudents(N) { }
void leave(int p) { locations.erase(p); }
int seat()
{
    int dist = 0, studentToSit = 0;
    if (!locations.empty())
    {
        auto beg = locations.begin();
        if (*beg != 0) dist = *beg;
        for (auto it = next(beg); it != locations.end(); ++it)
        {
            int curDist = (*it - *beg)/2;
            if (curDist > dist) dist = curDist, studentToSit = *beg + dist;
            beg = it;
        }
        beg = prev(locations.end());
        if (numOfStudents - *beg - 1 > dist) studentToSit = numOfStudents-1;
    }
    locations.insert(studentToSit);
    return studentToSit;
}

// O(logN) seat and O(logN) leave
int n;
set<int> cur;                                           // stores point at which student is placed
set<pair<int, int>, greater<pair<int, int>>> cand;      // stores [min_dist, -point] pair for possible future candidates. Pick one with highest distance and lowest index.
ExamRoom(int N) : n(N) { }
void addCandidate(int a, int b)
{
    int nextInd = (a + b)/2;
    if (nextInd != a) cand.emplace(min(abs(b - nextInd), abs(a - nextInd)), -nextInd);
}
void deleteCandidate(int a, int b)
{
    int nextInd = (a + b)/2;
    if (nextInd != a) cand.erase({min(abs(b - nextInd), abs(a - nextInd)), -nextInd});
}

int seat()
{
    if (cur.empty()) { cur.insert(0); cand.emplace(n-1, -(n-1)); return 0; }
    // Place student on highest distance i.e. begin of cand, then placing new means breaking candidate area into two so adding them
    auto [len, ind] = *cand.begin();
    ind = -ind;
    cand.erase(cand.begin());
    auto nxt = cur.upper_bound(ind);
    if (nxt != cur.end()) addCandidate(ind, *nxt);
    auto prev = cur.upper_bound(ind);
    if (prev != cur.begin()) { prev--; addCandidate(*prev, ind); }
    cur.insert(ind);
    return ind;
}
void leave(int p)
{
    cur.erase(p);
    if (cur.empty()) { cand.clear(); return; }
    int nextInd = -1, prevInd = -1;
    auto nxt = cur.upper_bound(p);
    if (nxt != cur.end()) { nextInd = *nxt; deleteCandidate(p, *nxt); }
    auto prev = cur.upper_bound(p);
    if (prev != cur.begin()) { prev--, prevInd = *prev; deleteCandidate(*prev, p); }
    if (cur.size() == 1)
    {
        cand.clear();
        int ind = *cur.begin();
        if (ind != 0) cand.emplace(ind, 0);
        if (ind != n-1) cand.emplace(n-1-ind, -(n-1));
        return;
    }
    if (p == 0) cand.emplace(*cur.begin(), 0);
    else if (p == n-1) cand.emplace(n-1-*cur.rbegin(), -(n-1));
    else if (nextInd >= 0 && prevInd >= 0) addCandidate(prevInd, nextInd);
}
```

### Minimum in rotated sorted array

```cpp
int findMin(vector<int> &nums)
{
    int l = 0, r = nums.size() - 1;
    while (l < r)
    {
        if (nums[l] < nums[r])
            return nums[l];
        int mid = l + (r - l) / 2;
        if (nums[mid] > nums[r]) l = mid + 1;
        else r = mid;
        // In case of equal instead of else, these 2 lines:
        else if (nums[mid] < nums[l]) r = mid, l++;
        else r--;
    }
    return nums[l];
}
```

### [Matrix Median](https://www.interviewbit.com/problems/matrix-median/)

```cpp
/* Binary search from min to max element of the matrix
upper bound works on sorted array only it tells how many
elements are lesser then or equal to specified Toh we check
mid se kam kitne elements he humeshaa
[1, 3, 5][2, 6, 9][3, 6, 9]
l = 1   r = 9   mid = 5
cur = 3 + 1 + 1 = 5
l = 1   r = 5   mid = 3
cur = 2 + 1 + 1 = 1
l = 4   r = 5   mid = 4
cur = 2 + 1 + 1 = 4
l = 5, r = 5 = ans 5(n*m+1)/2 is 5 which signifies upperbound
must be 5 for median but if matrix is like [[1, 1, 3, 3, 3, 3]]
our check for upperbound of 3 will not be (m*n+1)/2 although
it is clearly the median. But it clear if cur is greater then
count then our answer must be from l to mid and reverse in vice versa. */

int Solution::findMedian(vector<vector<int>> &A)
{
    int l = INT_MAX, r = INT_MIN;
    for (auto x : A)
        for (int i : x)
            l = min(l, i), r = max(r, i);
    while (l < r)
    {
        int mid = l + (r - l) / 2;
        int places = 0;
        for (auto x : A)
            places += upper_bound(x.begin(), x.end(), mid) - x.begin();
        if (places < ((A.size() * A[0].size()) + 1) / 2)
            l = mid + 1;
        else
            r = mid;
    }
    return l;
}
/* Suppose there's a very big array which cannot be stored as
one but only as 100 different arrays in sorted order each.
find the median of it as one collective array. The answer is to
treat it like a matrix and then simply do it. */
```

### [Median of two sorted arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

```cpp
class Solution {
public:
    double solve(const vector<int> &A, const vector<int> &B, int l, int r, int count)
    {
        while (l < r)
        {
            // using mid = (l+r)/2 can cause TLE in case [0,0,0,0,0] [-1,0,0,0,0,0,1]
            // because division of negative i.e. (l+r)/2 can act in reverse sense of floor
            // l + (r-l)/2 on the other hand divides positive r-l which is always positive
            int mid = l + (r-l)/2;
            int cnt = (upper_bound(A.begin(), A.end(), mid) - A.begin()) + 
                (upper_bound(B.begin(), B.end(), mid) - B.begin());
            if (cnt > count) r = mid;
            else l = mid+1;
        }
        return l;
    }
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2)
    {
        int n = nums1.size(), m = nums2.size();
        if (n == 0 && m == 0) return 0;
        else if (m == 0) return (n&1) ? nums1[n/2] : (nums1[n/2 - 1] + nums1[n/2])/(double)2;
        else if (n == 0) return (m&1) ? nums2[m/2] : (nums2[m/2 - 1] + nums2[m/2])/(double)2;
        
        int l = min(nums1[0], nums2[0]), r = max(nums1.back(), nums2.back());
        return ((n+m)&1) ? solve(nums1, nums2, l, r, (n+m)/2) :
            (solve(nums1, nums2, l, r, (n+m)/2 - 1) + solve(nums1, nums2, l, r, (n+m)/2)) / (double)2;
    }
};
```

### [Painter's Partition Problem](https://www.interviewbit.com/problems/painters-partition-problem/)

```cpp
/* Given boards and we have 3 painters: 100 200 300 400 500 | 600 700 | 800 900
This way each painter would have to: 1500 | 1300 | 1700 so variance is less
Way like calculating total paint and then dividing by k and trying to match
each partition as close to it could not evaluate all possibilities systematically
and thus do not always produce the correct solution. */
int partition(vector<int> &arr, int &k, int cur = 0, int cnt = 0)
{
    if (cnt > k) return INT_MAX;
    if (cur == arr.size())
        return (cnt == k ? 0 : INT_MAX);
    int sm = 0, res = INT_MAX;
    for (int i = cur; i < arr.size(); ++i)
    {
        sm += arr[i];
        res = min(res, max(sm, partition(arr, k, i+1, cnt+1)));
    }
    return res;
}
// It's O(K N^2) after memoization

// Binary Search Approach O(N log(sum of arr))
/* example - [1, 2, 3, 4, 5, 6, 7, 8, 9]
9-------45
low     high
low is when there are n painters. high is when there is 1 painter
find mid = 27 find what mid corresponds to 1+2+3+4+5+6 and 7+8+9 this way array partitions
such that parts sum is less then mid. (2 parts)
6------2------1
low    mid   high
apply binary search on low and mid */

#define ll long long
bool isPossible(int painters, vector<int> &boards, int val)
{
    ll count = 1, sum = val, i = 0;
    while (i < boards.size())
    {
        if (sum - boards[i] < 0)
            sum = val, count++;
        else
            sum -= boards[i++];
        if (count > painters)
            return false;
    }
    return true;
}
int Solution::paint(int A, int B, vector<int> &C)
{
    ll l = 0, r = accumulate(C.begin(), C.end(), 0);
    while (l < r)
    {
        int mid = l + (r - l) / 2;
        if (isPossible(A, C, mid))
            r = mid;
        else
            l = mid + 1;
    }
    return (l * B) % 10000003;
}
```

### [Minimize Max Distance to Gas Station](https://www.lintcode.com/problem/minimize-max-distance-to-gas-station/description)
```c++
double minmaxGasDist(vector<int> &stations, int k)
{
    const double EPS = 1e-6;
    auto check = [&](double x)
    {
        int cnt = 0;
        for (int i = 1; i < stations.size(); ++i)
        {
            cnt += ceil((double)(stations[i] - stations[i-1]) / x) - 1;
            if (cnt > k) return false;
        }
        return (cnt <= k);
    };
    
    double l = 0, r = EPS;
    while (!check(r)) r *= 2;
    while (l + EPS < r)
    {
        double mid = (l + r)/2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return r;
}
```

## Codeforces

* [Maximum Median](https://codeforces.com/contest/1201/problem/C): [https://codeforces.com/contest/1201/submission/76923324](https://codeforces.com/contest/1201/submission/76923324)
* [Mafia:](https://codeforces.com/contest/348/problem/A) In a game in one round there is 1 moderator and rest n-1 people are player. Given an array of n people signifying times they want to be player. Tell minimum number of games they must play to fulfil that condition of atleast playing ai times. **Solution:** [https://codeforces.com/contest/348/submission/76647058](https://codeforces.com/contest/348/submission/76647058)
* [Multiplication Table](https://codeforces.com/contest/448/problem/D) - Easy binary search, initially our search space l = 1, r = n\*m signifies our answer can be anything between 1 to n\*m we are searching over value. For every mid we find count of items by iterating over row and finding &lt;= mid count **Solution:** [https://codeforces.com/contest/448/submission/74697633](https://codeforces.com/contest/448/submission/74697633)
* [Hamburgers](https://codeforces.com/problemset/problem/371/C) - Given a string denoting recipe of hamburger B means bun S means Sausage and C means cheese in order L to R. We already have nb ns nc amount already and can purchase addition at cost pb ps pc. What is maximum hamburger we can make given we have r amount. **Solution**: n\[i\] + x = cur \* cnt\[i\], here cnt is cnt of that item required and x is additional item we want. We want to make cur amount of hamburger and want to see if it's possible. x = \(cur\*cnt\[i\] - n\[i\]\) \* p\[i\] will give additional cost we have to spend. Take care of case when that item is not at all reqd \(cnt\[i\] = 0\) and difference is negative. Finally also take care of binary search range, you cant make r 1e18 otherwise calculation will give ll overflow. [https://codeforces.com/contest/371/submission/74009612](https://codeforces.com/contest/371/submission/74009612)
* [Present](https://codeforces.com/contest/460/problem/C) - Given 3 integers n, m, w and an array of n numbers denoting height of ith plant. We can choose a subsegment of length w and increase height by 1 it will require one operation \(-1 of m\) find maximal \(minimum height\) across array we can get. **Solution:** Apply binary search on value \(x means we can form that maximal minimum height\) Now check if it's possible. This part is main, reqd holds how much operation that element required and cur is carry over operation from previous subsegment. Pure greedy strategy. [https://codeforces.com/contest/460/submission/76685479](https://codeforces.com/contest/460/submission/76685479)
* [Renting Bikes](https://codeforces.com/contest/363/problem/D) - Given n, m, a : denoting n peoples m bikes available for renting and a \(shared money they have\) An array b denotes personal money ith person has. Another array p represents price of bike. What minimum sum of personal money will they have to spend in total to let as many schoolchildren ride bikes as possible? **Solution:** Greedy binary search type of problem. Check if we can give x people bikes \(sort bike based on price in asc while people in desc based on personal money\) Idea is for x person to have bike xth richest person must be able to afford it \(that's why such sorting\) then simple greedy check function. [https://codeforces.com/contest/363/submission/76699115](https://codeforces.com/contest/363/submission/76699115)
* [One-Dimensional Battle Ships](https://codeforces.com/contest/567/problem/D) - Given a 1 x n matrix where Alice placed k ships each of width a, there must be a gap of atleast 1 within each ship. Given m queries which bob will ask and Alice will tell if that place is a hit or miss, Alice always cheats and keep telling bob it's a miss find the first point at which Bob can tell Alice is lying. **Solution:** Binary search over the point which is our optimal answer point and check if it's optimal by using a fill like approach of simple dp. [https://codeforces.com/contest/567/submission/76719058](https://codeforces.com/contest/567/submission/76719058)
* [Memory for Arrays](https://codeforces.com/contest/309/problem/C) - Really a nice problem, Given two arrays a and b of size n and m respectively. Array a denotes free space which needs to be allocated by task in array b. Array b is in bit \(so 3 means it require 8 space\) If there are two processes of 2, 2 \(means require 8-8 space\) one block of 16 can be used there. Find maximum task we can finish. **Solution:** Create a allocation table with rows 0, 1, 2.. means 2^0 can fit how many in given situation then we can apply greedy in reverse manner keeping track of used space \(multiplying it by 2\) [https://codeforces.com/contest/309/submission/76757279](https://codeforces.com/contest/309/submission/76757279)
* [Degenerate Matrix](https://codeforces.com/contest/549/problem/H) - Solution involve finding a point x \(using binary search\) such that a +- x, b +- x, c +- x, d +- x denotes new matrix which should give determinant zero and d should be minimal. Now in the code we return true if det = 0 but for &gt; 0 and &lt; 0 we mark a variable true that's because simple det=0 will detect peak point we want to if x gives true then x-1 also give true in that case our condition will hold [https://codeforces.com/contest/549/submission/76795248](https://codeforces.com/contest/549/submission/76795248)
* [R2D2 and Droid Army](https://codeforces.com/contest/514/problem/D) - Solution involves binary search over optimal length using maximum window search technique using deque in linear time over each of 5 columns to see if it's possible to pick such length. [https://codeforces.com/contest/514/submission/76804227](https://codeforces.com/contest/514/submission/76804227)
* [New Year Snowmen](https://codeforces.com/contest/140/problem/C) - Binary Search over pair\_count and maximize it. Sort our array in ascending order and inside check function distribute numbers \(taken linearly\) from array to mid x 3 ans matrix in column major form such that mat\[i\]\[0\] &lt; mat\[i\]\[1\] &lt; mat\[i\]\[2\] is satisfied for all i. Call check function last time before printing values since some false check might have changed ans values. [https://codeforces.com/contest/140/submission/76955210](https://codeforces.com/contest/140/submission/76955210)
* [K-Dominant Character](https://codeforces.com/contest/888/problem/C) - Binary search to minimize optimal subarray length, now the problem becomes given a length x find if all string subarrays of that length has anything in common. Maintain a table storing frequency of characters for window ending at i \(of length x\) In the end we just need to check for each char if all of them are &gt; 1 if none of 26 chars hold that that means there's nothing common. [https://codeforces.com/contest/888/submission/76990330](https://codeforces.com/contest/888/submission/76990330)
* [Road to Cinema](https://codeforces.com/contest/729/problem/C) - Binary search over volume \(from minVal-1 to maxVal+1 because while\(l+1 &lt; r\) searches from \(l, r\) exclusive\) We want to see if we can make it with minimum volume fuel tank and then later pick the one corresponding to minimum price for that volume. Our check function sees adjacent distance. If dist &gt; x i.e. distance is greater than available fuel we cannot reach otherwise min\(x, dist-x\) denotes minimum kilometers accelerated mode we used. So in the end checking it with time we have. [https://codeforces.com/contest/729/submission/77025244](https://codeforces.com/contest/729/submission/77025244)
* [Energy Exchange](https://codeforces.com/contest/68/problem/B) - Binary search over the final equalized values that we can make then check it by greedily iterating over desc order sorted array and performing operations. [https://codeforces.com/contest/68/submission/77031113](https://codeforces.com/contest/68/submission/77031113)
* [Three Base Stations](https://codeforces.com/contest/51/problem/C) - Binary search over the optimal value of d and try to minimize it, in check greedily place the tower right after \(+ d\) of an uncovered area. Do keep in mind that in `while (i < arr.size() && arr[i] < end)` Writing arr\[i\] &lt;= end would have given WA because of weird floting point precision thing in c++ so check = condition in next line using abs and EPS logic. [https://codeforces.com/contest/51/submission/77035869](https://codeforces.com/contest/51/submission/77035869)
* [Frodo and Pillows](https://codeforces.com/contest/760/problem/B) - [https://codeforces.com/contest/760/submission/77058869](https://codeforces.com/contest/760/submission/77058869)
* [String Game](https://codeforces.com/contest/779/problem/D) - [https://codeforces.com/contest/779/submission/77066382](https://codeforces.com/contest/779/submission/77066382)
* [Success Rate](https://codeforces.com/contest/807/problem/C) - Interesting problem, new x is k\*p same with y, new y = k\*q. Applying binary search on k \(on anything else will not lead to monotonicity\) x' = p\*k = x + succ i.e. succ = p\*k - x simmilarly y' = q\*k = y + succ + unsucc i.e. unsucc = q\*k - y - succ [https://codeforces.com/contest/807/submission/77076898](https://codeforces.com/contest/807/submission/77076898)
* [Motorack's Birthday](https://codeforces.com/problemset/problem/1301/B)

```cpp
/* Apply ternary search on values then replace every -1 with that
value and find corresponding adjacent element diff (we have to
minimize this) this function is bell shaped. */
int solve(vector<int> &arr, int x)
{
    int ans = 0;
    for (int i = 1; i < arr.size(); ++i)
    {
        int p = (arr[i-1] == -1) ? x : arr[i-1];
        int q = (arr[i] == -1) ? x : arr[i];
        ans = max(ans, abs(p-q));
    }
    return ans;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        vector<int> arr(n);
        for (auto &x : arr) cin >> x;
        int l = 0, r = 1e9;
        while (l+3 <= r)
        {
            int a = l+(r-l)/3, b = r-(r-l)/3;
            if (solve(arr, a) > solve(arr, b)) l = a;
            else r = b;
        }
        int ans = solve(arr, l), pt = l;
        for (int i = l+1; i <= r; ++i)
        {
            int cur = solve(arr, i);
            if (cur < ans) ans = cur, pt = i;
        }
        cout << ans << " " << pt << '\n';
    }
    return 0;
}
```

* [Mixing Water](https://codeforces.com/contest/1359/problem/C) - Given 3 values in each test case h, c, t. You can put hot cold hot cold order to minimize abs diff between final tmp and t.

```cpp
// Apply ternary search to minimize the function
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        double h, c, d; cin >> h >> c >> d;
        auto func = [&](double mid)
        {
            double q = (mid*h + mid*c + h) / (2*mid + 1);
            if (mid == 0) return fabs(q-d);
            double p = (mid*h + mid*c) / (2*mid);
            return min(fabs(p-d), fabs(q-d));
        };
 
        int l = 0, r = 1e13;
        while (l+1000 <= r)        
        {
            int a = l + (r-l)/3, b = r - (r-l)/3;
            if (func(a) > func(b)) l = a;
            else r = b;
        }
        double ans = func(l);
        int pt = l;
        for (int i = l+1; i <= r; ++i)
        {
            double cur = func(i);
            if (cur < ans) ans = cur, pt = i;
        }
    
        double q = (pt*h + pt*c + h) / (2*pt + 1);
        if (pt == 0) { cout << 2*pt+1 << "\n"; continue; }
        double p = (pt*h + pt*c) / (2*pt);
        if (fabs(p-d) < fabs(q-d)) cout << 2*pt << '\n';
        else cout << 2*pt+1 << '\n';
    }
    return 0;
}
```

