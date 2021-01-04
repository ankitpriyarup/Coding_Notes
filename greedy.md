# Greedy

## Why greedy coin problem works? \[TODO\]

## Scheduling Problems

Classic problem given start and end time pick max non overlapping schedules.  
Strategies: Pick smallest duration first \(fails\), pick that begins as early as possible \(fails\), pick that ends as early as possible

## Tasks and deadlines

Given duration and deadlines devise optimal schedule. For each task, we earn d-x points where d is the task's deadline and x is the moment when we finish it. What is largest possible total score.

![c: 5 points, b: 0 points, a: -7 points, d: -8 points making total -10](.gitbook/assets/image%20%2885%29.png)

Surprisingly, the optimal solution to the problem does not depend on the deadlines at all, but a correct greedy strategy is to simply perform the tasks sorted by their duration in increasing order. The reason for this is that if we ever perform two tasks one after another such that the first task takes longer than the second task, we can obtain a better solution if we swap the tasks.

## Minimizing sums

We are given n numbers a1, a2, ... an and we have to find a value x that minimizes the sum: \|a1 - x\|^c + \|a2 - x\|^c + .... + \|an - x\|^c

**Case \(c = 1\):** best choice is take x as median of array, because if x is smaller than the median, the sum becomes smaller by increasing x, and if x is larger then the median, the sum becomes smaller by decreasing x.  
**Case \(c = 2\):** best choice is take x as mean of array, because we can write it as:

![Ignore third term since it doesnt depend on x, we can see mean is important](.gitbook/assets/image%20%28191%29.png)

## Data Compression

{% embed url="https://youtu.be/co4\_ahEDCho" %}

## Reservoir Sampling

Reservoir Sampling is useful when there is an endless stream of data and your goal is to grab a small sample with uniform probability.

Given a sample of size k with n items processed so far, the chance for any item to be selected is k/n. When the next item comes in, current sample has a chance to survive k/\(n+1\). In general selection uniform random picking scenario k=1

## Problems

### Is Unique

Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structure?

```text
Clear if string is an ASCII or Unicode. Assuming ASCII it will have 128
characters make a bool array. Or do it bitwise (int array) - reducing space
by a factor of 8.

Without data structure:
- Compare every character with every other makes it O(N^2).
- We can sort and then do a linear scan.
- If it was a smaller range say 26 (a-z, we can resize our string to 26. And
  for each vis char mark it within string by incrementing char to some const.
```

### Check Permutation

Given two strings, write a method to decide if one is a permutation of the other.

```text
- Sort the string O(NlogN)
- Count item using a hashtable
```

### Valid Paranthesis String

Given a string containing only three types of characters: '(', ')' and '*'. * can be replaced by ( or ). Check if string is valid

```c++
// Recursive solution, easily can be memoized O(N^2)
bool checkValid(string &s, int cnt = 0, int cur = 0)
{
    if (cnt < 0)
    	return false;
    if (cur == s.size())
    	return (cnt == 0);

    if (s[cur] == '(')
    	return checkValid(s, cnt+1, cur+1);
    else if (s[cur] == ')')
    	return checkValid(s, cnt-1, cur+1);
    else
    {
    	return checkValid(s, cnt, cur+1) | checkValid(s, cnt+1, cur+1) |
    	checkValid(s, cnt-1, cur+1);
    }
}
bool checkValidString(string s)
{
    return checkValid(s);
}

// Greedy solution
bool checkValidString(string s)
{
    int l = 0, r = 0;
    for (char ch : s)
    {
        l += (ch == '(') ? 1 : -1;      // we are putting ) in place of *
        r += (ch == ')') ? -1 : 1;      // we are putting ( in place of *
        l = max(l, 0);                  // closing brackets cannot exceed than opening (no < 0 case)
        if (r < 0) break;               // In case of )
    }
    return (l == 0);
}
```

### [Excel Number to Title](https://leetcode.com/problems/excel-sheet-column-title/)

```cpp
class Solution {
public:
    string convertToTitle(int n)
    {
        string res;
        while (n)        
        {
            n -= 1;        // We want 1 to be A 26 to be Z 28 to be AB
            res = (char)('A' + (n%26)) + res;
            n /= 26;
        }
        return res;
    }
};
```

### Excel Title to Number

```cpp
class Solution {
public:
    int titleToNumber(string s)
    {
        int res = 0;
        for (const char ch : s)
            res = res*26 + (ch-'A'+1);
        return res;
    }
};
```

### URLify

Write a method to replace all spaces in a string with '%20'. You may assume that the string has sufficient space that end to hold the additional character, and that you are given the "true" length of the string.

```cpp
// start from end we have extra buffer in end, later we can trim if needed
void urlify(string &str, int trueLen)
{
    int j = str.size()-1;
    for (int i = trueLen-1; i >= 0; --i)
    {
        if (str[i] == ' ')
        {
            str[j] = '0', str[j-1] = '2', str[j-2] = '%';
            j -= 3;
        }
        else
        {
            str[j] = str[i];
            j--;
        }
    }
    str = str.substr(j+1);
}
```

### Palindrome Permutation

Given a string, write a function to check if it is a permutation of a palindrome.

```text
Check count of characters, %2 of them must be 0 except in case of odd length
string we can have one exception character.
```

### One Away

There are 3 types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings write a function to check if they are one edit \(or zero edits\) away.

```cpp
bool isOneEditDistance(string &s, string &t)
{
    if (s == t) return true;    // zero edit distance also allowed
    int n = s.size(), m = t.size();
    if (abs(n-m) > 1) return false;

    int count = 0, i, j;
    for (i = 0, j = 0; i < n, j < m;)
    {
        if (s[i] == t[j]) i++, j++;
        else
        {
            if (n > m) i++;
            else if (n < m) j++;
            else i++, j++;
            count++;
        }
        if (count > 1) return false;
    }
    if (i < n || j < m) count++;
    return count == 1;
}
```

### String Compression

Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase \(a-z\)

```cpp
string compress(string &str)
{
    if (str.empty()) return str;
    string res = "";
    int cnt = 1;
    for (int i = 1; i < str.size(); ++i)
    {
        if (str[i] == str[i-1]) cnt++;
        else
        {
            res += str[i-1] + to_string(cnt);
            cnt = 1;
        }
    }
    if (cnt) res += str.back() + to_string(cnt);
    return (res.size() < str.size()) ? res : str;
}
```

### Rotate Matrix

Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do it in place?

```cpp
void rotate(vector<vector<int>>& matrix)
{
    int n = matrix.size();
    for (int i = 0; i < n/2; ++i)
    {
        for (int j = i; j < n-1-i; ++j)
        {
            int a = matrix[i][j];
            int b = matrix[j][n-1-i];
            int c = matrix[n-1-i][n-1-j];
            int d = matrix[n-1-j][i];

            matrix[i][j] = d;
            matrix[j][n-1-i] = a;
            matrix[n-1-i][n-1-j] = b;
            matrix[n-1-j][i] = c;
        }
    }
}
```

### Zero Matrix

Write an algorithm such that if an element in an MxN Matrix is 0, its entire row and column are set to 0.

```text
Iterate on matrix, mark that row and column as 1 in a bool array
if value is 0. Later again iterate and see if that row or col is 1
then set it to 0.

Reduce space by a factor of 8 using bit manipulation.

To make O(1) space use first row and first col, set it to 0 marking
entire row/col zero.
```

### Substring Rotation

Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using only one call to isSubstring \(eg: "waterbottle" is rotation of "erbottlewat"\)

```text
For a rotation we have a rotation point
waterbottle
   ---y----
-x-
erbottlewat

s1 = xy
s2 = yx
simply do isSubstring(s1s1, s2)
this should give ans
```

### [Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/)

```cpp
// Implementation in linked list (stream of input)
ListNode *root;
Solution(ListNode* head) : root(head) { }
int getRandom()
{
    int res, len = 1;
    ListNode *cur = root;
    while (cur)
    {
        if (rand()%len == 0) res = cur->val;
        len++;
        cur = cur->next;
    }
    return res;
}
```

### [Weighted Reservoir Sampling](https://leetcode.com/problems/random-pick-with-weight/)

_Given an input list of values w = \[2, 8\], when we pick up a number out of it, the chance is that 8 times out of 10 we should pick the number at index 1 \(zero indexed\)_

```cpp
int pickIndex()
{
    int res = 0, sm = nums[0];
    for (int i = 1; i < nums.size(); ++i)
    {
        if (rand() % (sm + nums[i]) < nums[i]) res = i;
        sm += nums[i];
    }
    return res;
}
// TLE when nums val is larger, reservoir sampling is not for static type data
// it should be used when we are to introduce new elements
```

#### Other method involving prefix sum

```cpp
vector<int> prefSm;
Solution(vector<int>& w)
{
    prefSm.assign(w.size(), 0);
    prefSm[0] = w[0];
    for (int i = 1; i < w.size(); ++i)
        prefSm[i] = prefSm[i-1] + w[i];
}
int pickIndex()
{
    int x = rand()%prefSm.back();
    auto it = upper_bound(prefSm.begin(), prefSm.end(), x);
    return (it - prefSm.begin());
}
```

### [Next Permutation](https://www.nayuki.io/page/next-lexicographical-permutation-algorithm)

![](.gitbook/assets/image%20%28206%29.png)

```cpp
void nextPermutation(vector<int>& nums)
{
    int i = nums.size()-1;
    while (i > 0 && nums[i-1] >= nums[i]) i--;
    if (i > 0)
    {
        int j = nums.size()-1;
        while (nums[j] <= nums[i-1]) j--;
        int temp = nums[i-1];
        nums[i-1] = nums[j];
        nums[j] = temp;
    }
    reverse(nums.begin() + i, nums.end());
}
```

### [Container with most water](https://www.interviewbit.com/problems/container-with-most-water/)

```cpp
class Solution {
public:
    int maxArea(vector<int>& height)
    {
        int res = 0;
        for (int i = 0, j = height.size()-1; i < j;)
        {
            res = max(res, (j-i) * min(height[i], height[j]));
            height[i] < height[j] ? i++ : j--;
        }
        return res;
    }
};
```

Wow magic, right? Here's proof by contradiction

Suppose the returned result is not the optimal solution, then there must exist an optimal solution, say a container with al or ar \(left or right\) such that it has greater volume than the one we got. Since our algorithm stops only if the two pointers meet. So, we must have visited one of them but not the other let's say we visited al \(but not ar\).

So when a pointer stops at al, it won't move until:

* The other pointer also points to al, in that case iteration ends but the other pointer must have visited ar on its way from right end to al so contradicting our assumption that we didn't visit ar.
* The other pointer arrives at a value, say ar' that is greater than al before it reaches ar. In this case we does move al but notice that the volume of al and ar' is already greater than al and ar \(as it is wider and heighter\) which means that al and ar is not the optimal solution -- contradiction.

### [Trapping Rain Water](https://practice.geeksforgeeks.org/problems/trapping-rain-water/0)

```cpp
// Make two arrays one storing all maxLeft and one maxRight. min for each
// index excpet first and last sum gives final ans
using namespace std;

int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        int arr[n];
        for (int i = 0; i < n; ++i) cin >> arr[i];

        int maxLeft[n], maxRight[n];
        int curMax = 0;
        for (int i = 0; i < n; ++i)
        {
            maxLeft[i] = curMax;
            curMax = max(curMax, arr[i]);
        }
        curMax = 0;
        for (int i = n-1; i >= 0; --i)
        {
            maxRight[i] = curMax;
            curMax = max(curMax, arr[i]);
        }
        maxLeft[0] = -1, maxRight[n-1] = -1;

        int ans = 0;
        for (int i = 0; i < n; ++i)
        {
            if (maxLeft[i] != -1 && maxRight[i] != -1)
            {
                int val = min(maxLeft[i], maxRight[i]) - arr[i];
                if (val > 0) ans += val;
            }
        }
        cout << ans << endl;
    }
    return 0;
}

// O(1) space solution using 2 pointer
int trap(vector<int>& h)
{
    int n = h.size(), ans = 0, lm = 0, rm = 0,  l = 0, r = n-1;
    if (n <= 2) return 0;
    while (l <= r)
    {
        rm = max(rm, h[r]);
        lm = max(lm, h[l]);
        if (lm <= rm) ans += lm - h[l++]; 
        else ans += rm - h[r--];
    }
    return ans;
}
```

```cpp
class Solution {
public:
    typedef pair<int, int> pii;
    int trapRainWater(vector<vector<int>>& heightMap)
    {
        /* One quick approach is, start iterating from top left to bottom right creating dp[i][j]
        as min of dp[i-1][j] and dp[i][j-1] same create another dp from bottom right to top left
        dp[i][j] is min of dp[i+1][j] and dp[i][j+1]. For all internal points max of both dp - height
        amount of water will get stored.
        However it will fail in some cases, reason is simple that bottom left to top right flow is
        also important to maintain.
        
        So a multi-source BFS based solution, put all border elements as source initially. Move in
        all 4 directions keeping visited record.
        
        Go greedy and use min heap based on height to maintain min property from before. */
        if (heightMap.empty()) return 0;
        int n = heightMap.size(), m = heightMap[0].size();
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        vector<vector<bool>> visited(n, vector<bool>(m, false));
        for (int i = 0; i < n; ++i)
        {
            for (int j = 0; j < m; ++j)
            {
                if (i == 0 || i == n-1 || j == 0 || j == m-1)
                {
                    pq.push({heightMap[i][j], i*m + j});
                    visited[i][j] = true;
                }
            }
        }
        int mx = INT_MIN, res = 0;
        vector<pii> dir = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        while (!pq.empty())
        {
            auto cur = pq.top(); pq.pop();
            int height = cur.first, r = cur.second/m, c = cur.second%m;
            mx = max(mx, height);
            for (auto &x : dir)
            {
                int _r = r + x.first, _c = c + x.second;
                if (_r < n && _r >= 0 && _c < m && _c >= 0 && !visited[_r][_c])
                {
                    visited[_r][_c] = true;
                    if (heightMap[_r][_c] < mx) res += (mx - heightMap[_r][_c]);
                    pq.push({heightMap[_r][_c], _r*m + _c});
                }
            }
        }
        return res;
    }
};
```

### Kth Element After Merging 2 Sorted Arrays

```cpp
/*	[1, 2, 3]		[2, 4]
		k = 4 will give 3 in log(k) time */
int solve(vector<int> &a, vector<int> &b, int k, int cur1 = 0, int cur2 = 0)
{
		int n = a.size()-cur1, m = b.size()-cur2;
		if (k > (n+m) || k < 1) return -1;
		if (m < n) return solve(b, a, k, cur2, cur1);		// a should be small then b

		if (n == 0) return b[cur2+k-1];				// a is empty now then pick rem from b
		if (k == 1) return min(a[cur1], b[cur2]);		// if only 1 to choose pick min

		/* Try to take half of rem from both a & b, if that point in a is small take
		it otherwise take from b */
		int i = min(n, k/2), j = min(m, k/2);
		if (a[cur1+i-1] > b[cur2+j-1])
				return solve(a, b, k-j, cur1, cur2+j);
		else
				return solve(a, b, k-i, cur1+i, cur2);
}
```

* Extension: [https://leetcode.com/problems/median-of-two-sorted-arrays/](https://leetcode.com/problems/median-of-two-sorted-arrays/)

### Maximum Continuous 1s after B flips

```cpp
vector<int> Solution::maxone(vector<int> &A, int B)
{
    int i = 0, j = 0, count = 0, bestTillNow = INT_MIN, ansI, ansJ;
    while (i < A.size())
    {
        if (count <= B)
        {
            if (A[i] == 0) ++count;
            ++i;
        }
        if (count > B)
        {
            if (A[j] == 0) --count;
            ++j;
        }
        if (i-j+1 > bestTillNow) bestTillNow = i-j+1, ansI = i, ansJ = j;
    }
    vector<int> res;
    if (bestTillNow == INT_MIN) return res;
    for (int i = ansJ; i < ansI; ++i) res.push_back(i);
    return res;
}
```

### Check if any permutation of a large number is divisible by 8

```cpp
bool check(string n, int l)
{
    if (l < 3)
    {
        if (stoi(n)%8 == 0) return true;
        reverse(n.begin(), n.end());
        if (stoi(n)%8 == 0) return true;
        return false;
    }
    int hash[10] = {0};
    for (int i = 0; i < l; ++i) hash[n[i] - '0']++;
    for (int i = 104; i < 1000; i += 8)
    {
        int dup = i, freq[10] = {0};
        freq[dup % 10]++;
        dup /= 10;
        freq[dup % 10]++; 
        dup /= 10; 
        freq[dup % 10]++; 
        dup = i;

        if (freq[dup % 10] > hash[dup % 10]) continue;
        dup = dup / 10;
        if (freq[dup % 10] > hash[dup % 10]) continue;
        dup = dup / 10;
        if (freq[dup % 10] > hash[dup % 10]) continue;
        return true;
    }
    return false;
}
```

### [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)

```cpp
int lastStoneWeight(vector<int>& stones)
{
    multiset<int> st;
    for (auto &x : stones) st.insert(x);
    while (st.size() > 1)
    {
        int p = *st.rbegin(); st.erase(--st.end());
        int q = *st.rbegin(); st.erase(--st.end());
        int cur = abs(p-q);
        if (cur != 0) st.insert(cur);
    }
    return *st.begin();
}
```

### Subarray with given sum

```cpp
using namespace std;
#define ll long long

int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n, s;
        cin >> n >> s;
        int arr[n];
        for (int i = 0; i < n; ++i) cin >> arr[i];

        int curr_sum = arr[0], j = 0;
        bool found = false;
        for (int i = 1; i <= n; ++i)
        {
            while (curr_sum > s) curr_sum -= arr[j++];
            if (curr_sum == s)
            {
                cout << j+1 << " " << i << endl;
                found = true;
                break;
            }
            if (i < n) curr_sum += arr[i];
        }
        if (!found) cout << -1 << endl;
    }
    return 0;
}
```

### [Move Zeroes](https://leetcode.com/problems/move-zeroes/)

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums)
    {
        int n = nums.size();
        for (int i = 0, j = 0; i < n; ++i)
        {
            if (nums[i] != 0)
                swap(nums[i], nums[j++]);
        }
    }
};
```

### [Sort Colors](https://leetcode.com/problems/sort-colors)

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums)
    {
        for (int i = 0, l = 0, r = nums.size()-1; i <= r;)
        {
            if (nums[i] == 0) swap(nums[i++], nums[l++]);
            else if (nums[i] == 2) swap(nums[i], nums[r--]);
            else i++;
        }
    }
};
```

### Majority Element - Voting Algorithm

```cpp
int Solution::majorityElement(const vector<int> &A)
{
    int ans = 0, x = 0;
    for (int i : A)
    {
        if (x == 0) ans = i;
        if (ans == i) x++;
        else x--;
    }
    return ans;
}
```

### N/3 Repeat Numbers

```cpp
int Solution::repeatedNumber(const vector<int> &A)
{
    int len = A.size();
    if (A.size() == 0) return -1;
    if (A.size() == 1) return A[0];
​
    int c1 = A[0];
    int c2 = A[1];
    int c1count = 0;
    int c2count = 0;
​
    for (int num : A)
    {
        if (c1 == num) c1count++;
        else if (c2 == num) c2count++;
        else if (c1count == 0)
        {
            c1 = num;
            c1count = 1;
        }
        else if (c2count == 0)
        {
            c2 = num;
            c2count = 1;
        }
        else
        {
            c1count--;
            c2count--;
        }
    }
​
    // Need to properly find count once again
    c1count = 0;
    c2count = 0;
    for (int num : A)
    {
        if (c1 == num) c1count++;
        else if (num == c2) c2count++;
    }
​
    if (c1count > len / 3) return c1;
    else if (c2count > len / 3) return c2;
    else return -1;
}
```

### Triplet with given product

```cpp
int countTriplets(int arr[], int m, int n)
{
    unordered_map<int, int> freq;
    set< int, pair<int, int> > res;
    for (int i : arr) freq[i]++;
    
    // Iterate till root m if it is divisible by m and present in arr take it
    for (int i = 1; i * i <= m; ++i)
    {
        if (m % i == 0 && freq[i])
        {
            int num1 = m/i;
            for (int j = 1; j * j <= num1; ++j)
            {
                if (num1 % j == 0 && freq[j])
                {
                    int num2 = num1/j;
                    if (freq[num2])
                    {
                        // Put {num2, i, j}
                        int temp[2] = {num2, i, j};
                        sort(temp, temp+n);
                        int prevSize = res.size();
                        res.insert({temp[0}, {temp[1], temp[2]});
                        // A new triplet is found
                        if (res.size() != prevSize)
                        {
                            // If all no. in triplet are unique
                            if (i != j && j != num2)
                                ans += freq[i] * freq[j] * freq[num2];
                            else if (i == j && j != num2)
                                ans += (freq[j] * (freq[j]-1) / 2) * freq[num2];
                            else if (j == num2 && j != i)
                                ans += (freq[j] * (freq[j]-1) / 2) * freq[i];
                            else if (i == num2 && j != i)
                                ans += (freq[i] * (freq[i]-1) / 2) * freq[j];
                            else if (i == j && j == num2)
                                ans += (freq[i] * (freq[i]-1) * (freq[i]-2) / 6;
                        }
                    }
                }
            }
        }
    }
}
```

### Longest Subarray with average greater than x

```cpp
int maxSubarray(int arr[], int x, int n)
{
    int length = 0, sum = 0, temp = 0, max = 0;
    for (int i = 0; i < n; ++i)
    {
        sum += arr[i];
        temp++;
        if (sum > max && sum/temp >= x && temp >= length)
            max = sum, length = temp;
        if (sum < 0) sum = 0, temp = 0;
    }
    return length;
}
```

### [Fraction to Recurring Decimal](https://leetcode.com/problems/fraction-to-recurring-decimal/)

```cpp
string fractionToDecimal(int n, int d) {
    if (n == 0) return "0";
    if (d == 0) return "";

    long long num = n, den = d;

    string res = "";
    if ((num < 0) ^ (den < 0)) res += "-";
    num = abs(num), den = abs(den);
    long long quo = num/den;
    res += to_string(quo);
    long long rem = (num % den) * 10;
    if (rem == 0) return res;
    res += ".";

    // Calculate decimal values
    unordered_map<long long, int> rec;
    while (rem != 0)
    {
        if (rec.find(rem) != rec.end())
        {
            int beg = rec[rem];
            string part1 = res.substr(0, beg);
            string part2 = res.substr(beg, res.size());
            res = part1 + "(" + part2 + ")";
            return res;
        }
        rec[rem] = res.size();
        long long temp = rem/den;
        rem = (rem % den) * 10;
        res += to_string(temp);
    }
    return res;
}
```

### Integer to Roman Number

```cpp
string intToRoman(int num) 
{
    string res;
    string sym[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
    int val[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    
    for(int i=0; num != 0; i++)
    {
        while(num >= val[i])
        {
            num -= val[i];
            res += sym[i];
        }
    }
    
    return res;
}
```

### Roman to Integer Number

```cpp
int romanToInt(string s) {
    unordered_map<char, int> rec;
    rec['I'] = 1;
    rec['V'] = 5;
    rec['X'] = 10;
    rec['L'] = 50;
    rec['C'] = 100;
    rec['D'] = 500;
    rec['M'] = 1000;
    int sum = rec[s.back()];
    for (int i = s.size()-2; i >= 0; --i)
    {
        if (rec[s[i]] < rec[s[i+1]]) sum -= rec[s[i]];
        else sum += rec[s[i]];
    }
    return sum;
}
```

### Rectangles Overlap/Intersection

```cpp
bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2)
{
    int x1 = rec1[0], y1 = rec1[1], x2 = rec1[2], y2 = rec1[3];
    int x3 = rec2[0], y3 = rec2[1], x4 = rec2[2], y4 = rec2[3];
    // using pos
    return !(x2 <= x3 || y2 <= y3 || x1 >= x4 || y1 >= y4);
    // using area
    return (min(x2, x4) > max(x3, x1)) && (min(y2, y4) > max(y3, y1));
}
```

### Circle and Rectangle Overlap/Intersection

```cpp
bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2)
{
    int rectX = x1, rectY = y1, rectWidth = abs(x2-x1), rectHeight = abs(y2-y1);
                   // (These max terms are closest x and y points to circle from rectangle
    int deltaX = x_center - max(rectX, min(x_center, rectX + rectWidth));
    int deltaY = y_center - max(rectY, min(y_center, rectY + rectHeight));
    return (deltaX*deltaX + deltaY*deltaY) <= (radius*radius);
}
```

### Binary to Gray Code & Gray to Binary Code

```cpp
/*
Binary To Gray
    0011 -> 0010
    Take MSB as it is, rest are XOR wiht prev
Gray To Binary
    Take MSB as it is, rest if Gray bit is 0 take previous binary bit
    else if 1 then negate it
*/
// Get gray code vector when n bits are there
vector<int> grayCode(int n)
{
    vector<int> res(1, 0);
    for (int i = 1; i < (1<<n); ++i)
        res.push_back(res[i-1] ^ (i & -i));
    return res;
}
```

### [Degree of an Array](https://leetcode.com/problems/degree-of-an-array/)

```cpp
int findShortestSubArray(vector<int>& nums)
{
    unordered_map<int, int> cnt, firstTimeAppeared, lastTimeAppeared;
    for (int i = 0; i < nums.size(); ++i)
    {
        cnt[nums[i]]++;
        if (firstTimeAppeared.find(nums[i]) == firstTimeAppeared.end())
            firstTimeAppeared[nums[i]] = i;
        lastTimeAppeared[nums[i]] = i;
    }
    int bestDeg = 0;
    for (auto &x : cnt)
        bestDeg = max(bestDeg, x.second);

    int res = nums.size();
    for (auto &x : cnt)
    {
        if (x.second != bestDeg) continue;
        res = min(res, lastTimeAppeared[x.first] - firstTimeAppeared[x.first] + 1);
    }   
    return res;
}
```

### [Kth Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)

```cpp
int fact(int n)
{
    if (n > 12) return INT_MAX;
    int f = 1;
    for (auto i = 2; i<=n; ++i) f *= i;
    return f;
}

string Solution::getPermutation(int n, int k)
{
    vector<int> nums;
    for (int i = 1; i <= n; ++i) nums.push_back(i);
    string res = "";
    k--;
    while (n--)
    {
        int f = fact(nums.size()-1);
        int pos = k / f;
        k %= f;
        res += to_string(nums[pos]);
        nums.erase(nums.begin() + pos);
    }
    return res;
}
// Memoize factorial to save time in recalculating
```

### 4 Partition Problem

Given an array, we need to partition it into 4 contiguous subarray such that difference of maximum and minimum \(sum of subarrays\) is minimized.

```cpp
/*
5
3 2 4 1 2

If we divide A as B,C,D,E=(3),(2),(4),(1,2), then P=3,Q=2,R=4,S=1+2=3.
Here, the maximum and the minimum among P,Q,R,S are 4 and 2, with
the absolute difference of 2. We cannot make the absolute difference
of the maximum and the minimum less than 2, so the answer is 2.

Approach:
0    3    2    4    1    2
0    3    5    9   10   12
     l    i    r
  [3]    [2]  [4]     [3]
*/
signed main()
{
    flash;
    int n;
    cin >> n;
    int a[n+1] {}, b[n+1] {};
    for (int i = 1; i <= n; ++i)
    {
        cin >> a[i];
        b[i] = b[i-1] + a[i];
    }
    int l = 1, r = 3, x = 0, y = 0, z = 0, w = 0, ans = INF;
    for (int i = 2; i < n-1; ++i)
    {
        while (l < i && abs(b[l] - b[0] - (b[i] - b[l])) >= abs(b[l + 1] - b[0] - (b[i] - b[l + 1])))
            l++;
        while (r < n && abs(b[r] - b[i] - (b[n] - b[r])) >= abs(b[r+1] - b[i] - (b[n] - b[r + 1])))
            r++;
        x = b[l] - b[0];
        y = b[i] - b[l];
        z = b[r] - b[i];
        w = b[n] - b[r];
        ans = min(ans, max({x, y, z, w}) - min({x, y, z, w}));
    }
    cout << ans << '\n';
    return 0;
}
```

### [First Non Repeating Character in Stream](https://practice.geeksforgeeks.org/problems/first-non-repeating-character-in-a-stream/0)

```cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long

int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        char arr[n];
        for (int i = 0; i < n; ++i) cin >> arr[i];
        unordered_map<char, int> freq;

        queue<char> q;
        for (int i = 0; i < n; ++i)
        {
            freq[arr[i]]++;
            q.push(arr[i]);
            while (!q.empty())
            {
                if (freq[q.front()] > 1) q.pop();
                else
                {
                    cout << q.front() << " ";
                    break;
                }
            }
            if (q.empty()) cout << -1 << " ";
        }
        cout << endl;
    }
    return 0;
}
```

### Circular Tour

```cpp
int Solution::canCompleteCircuit(const vector<int> &gas, const vector<int> &cost)
{
    int fuel = 0, start_i = 0, sum = 0;
    for (int i = 0; i < gas.size(); ++i)
    {
        sum += (gas[i] - cost[i]);
        fuel += (gas[i] - cost[i]);
        if (fuel < 0) fuel = 0, start_i = i+1;
    }
    return (sum >= 0) ? start_i : -1;
}
```

### [Product Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

```cpp
// without using additional space
vector<int> productExceptSelf(vector<int>& nums)
{
    int n = nums.size();
    vector<int> res(n);
    res[0] = 1;
    for (int i = 1, cur = nums[0]; i < n; cur *= nums[i++])
        res[i] = cur;
    for (int i = n-2, cur = nums[n-1]; i >= 0; cur *= nums[i--])
        res[i] *= cur;
    return res;
}
```

### [Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

```cpp
int rangeBitwiseAnd(int m, int n)
{
    if (max(m, n) == 0) return 0;
    int ans = 0;
    for (int i = log2(max(m, n)); i >= 0; --i)
    {
        if ((m&(1<<i)) ^ (n&(1<<i))) return ans;
        else ans += (m&(1<<i));
    }
    return ans;
}
```

### [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

```cpp

```

### [Reaching Points](https://leetcode.com/problems/reaching-points/)

Given \(sx, sy\) in one move we can change \(x, y\) -&gt; \(x+y, y\) or \(x, x+y\)

```cpp
class Solution {
public:
    bool reachingPoints(int &sx, int &sy, int tx, int ty)
    {
        if (sx > tx || sy > ty) return false;
        if (sx == tx) return ((ty-sy+sx)%sx == 0);
        if (sy == ty) return ((tx-sx+sy)%sy == 0);
        return reachingPoints(sx, sy, tx-ty, ty) ||
            reachingPoints(sx, sy, tx, ty-tx);
    }
};
```

### [Best Meeting Point](https://www.lintcode.com/problem/best-meeting-point/description)

```cpp
/* Find median, we can also do multisource BFS that will
also give N^2 but it will be complex and have more space
complexity */
class Solution {
public:
    int minTotalDistance(vector<vector<int>> &grid)
    {
        vector<int> rows, cols;
        for (int i = 0; i < grid.size(); ++i)
            for (int j = 0; j < grid[0].size(); ++j)
                if (grid[i][j] == 1) rows.push_back(i), cols.push_back(j);
        
        sort(rows.begin(), rows.end());
        sort(cols.begin(), cols.end());

        int ans = 0;
        for (auto &x : rows)
            ans += abs(x - rows[rows.size()/2]);
        for (auto &x : cols)
            ans += abs(x - cols[cols.size()/2]);
        return ans;
    }
};
```

### [Max Points On a Line](https://leetcode.com/problems/max-points-on-a-line/)

```cpp
class Solution {
public:
    int maxPoints(vector<vector<int>>& points)
    {
        if (points.empty()) return 0;

        map<pair<int, int>, int> rec;
        int res = 1;
        for (int i = 0; i < points.size()-1; ++i)
        {
            int duplicates = 0, verticals = 0;
            for (int j = i+1; j < points.size(); ++j)
            {
                if (points[i][0] == points[j][0] && points[i][1] == points[j][1])
                    ++duplicates;
                else if (points[i][0] == points[j][0])
                    ++verticals;
                else
                {
                    int dx = points[j][0] - points[i][0];
                    int dy = points[j][1] - points[i][1];
                    int gcd = __gcd(dx, dy);
                    rec[{dx/gcd, dy/gcd}]++;
                }
            }

            res = max({res, duplicates+1, duplicates+verticals+1});
            for (auto &x : rec)
                res = max(res, x.second + duplicates + 1);
            rec.clear();
        }
        return res;
    }
};
```

### [Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle/)

Perform the instruction in given instruction and prepare final vector \(position+direction\) out of it. Answer is always false if direction comes out to be north and x-y are non zero

```cpp
class Solution {
public:
    bool isRobotBounded(string instructions)
    {
        int dir = 0;    // [0=North, 1=East, 2=South, 3=West]
        int x = 0, y = 0;
        const int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0};
        for (auto &ch : instructions)
        {
            if (ch == 'G') x += dx[dir], y += dy[dir];
            else dir = (dir + (ch == 'L' ? -1 : 1) + 4) % 4;
        }
        return !(dir == 0 && (x != 0 || y != 0));
    }
};
```

### [Reorganize String](https://leetcode.com/problems/reorganize-string/)

Reorganize string such that same characters are not adjacent.

Using greedy strategy and always put that char having higher count.

```cpp
class Solution {
public:
    string reorganizeString(string S)
    {
        unordered_map<char, int> cnt;
        for (auto &x : S) cnt[x]++;
        priority_queue<pair<int, char>> pq;
        for (auto &x : cnt) pq.push({x.second, x.first});

        string res = "";
        while (!pq.empty())
        {
            auto x = pq.top(); pq.pop();
            // If highest cnt char is same as prev then pop another
            if (res.size() > 0 && x.second == res.back())
            {
                if (pq.empty()) return "";
                auto y = pq.top(); pq.pop();
                res += y.second;
                y.first--;
                if (y.first) pq.push(y);
                pq.push(x);
            }
            else
            {
                res += x.second;
                x.first--;
                if (x.first) pq.push(x);
            }
        }
        return res;
    }
};
```

### [Find The Celebrity](https://www.lintcode.com/problem/find-the-celebrity/description)
Goal is to optimize calls to knows function
```c++
// N^2 calls
int findCelebrity(int n)
{
    vector<int> inDeg(n), outDeg(n);
    for (int i = 0; i < n; ++i)
    {
        for (int j = 0; j < n; ++j)
            if (i != j && knows(i, j)) outDeg[i]++, inDeg[j]++;
    }

    for (int i = 0; i < n; ++i)
        if (inDeg[i] == n-1 && outDeg[i] == 0) return i;
    return -1;
}

// N Calls
int findCelebrity(int n)
{
    int i = 0, j = n-1;
    while (i < j)
    {
        if (knows(i, j)) i++;
        else j--;
    }
    // Check if i(or j both are same) is actually celebrity or not
    for (int x = 0; x < n; ++x)
        if (x != i && (knows(i, x) || !knows(x, i))) return -1;
    return i;
}
```

### [Maximum Sum of Two Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/)
```c++
int maxSumTwoNoOverlap(vector<int>& A, int L, int M)
{
    int n = A.size();
    vector<int> pre(n, 0);
    pre[0] = A[0];
    for (int i = 1; i < n; ++i) pre[i] = A[i] + pre[i-1];
    
    // calculate prefix sum of all windows of size L and M starting at i
    vector<int> lPre(n, 0), mPre(n, 0);
    lPre[0] = pre[L-1], mPre[0] = pre[M-1];
    for (int i = 1; i < n-L+1; ++i) lPre[i] = pre[i+L-1] - pre[i-1];
    for (int i = 1; i < n-M+1; ++i) mPre[i] = pre[i+M-1] - pre[i-1];

    int res = INT_MIN;
    int mPreMax = mPre[0];
    for (int i = M, j = 0; i < n-L+1; ++i, ++j)     // M_group then L_group, maintain max of left one
        mPreMax = max(mPreMax, mPre[j]), res = max(res, lPre[i] + mPreMax);
    int lPreMax = lPre[0];
    for (int i = L, j = 0; i < n-M+1; ++i, ++j)     // L_group then M_group, maintain max of left one
        lPreMax = max(lPreMax, lPre[j]), res = max(res, mPre[i] + lPreMax);
    return res;
}
```

### [Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)
```c++
bool isPossible(vector<int>& nums)
{
    unordered_map<int, int> cnt, end;
    for (const int x : nums) cnt[x]++;
    for (const int x : nums)
    {
        if (cnt[x] <= 0) continue;
        if (end[x-1] > 0) cnt[x]--, end[x-1]--, end[x]++;
        else if (cnt[x+1] > 0 && cnt[x+2] > 0) cnt[x]--, cnt[x+1]--, cnt[x+2]--, end[x+2]++;
        else return false;
    }
    return true;
}
```

### [Min Area Rectangle](https://leetcode.com/problems/minimum-area-rectangle/)
```c++
int minAreaRect(vector<vector<int>>& points)
{
    unordered_map<int, unordered_set<int>> rec;
    for (const auto x : points)
        rec[x[0]].insert(x[1]);
    int res = INT_MAX;
    for (int i = 0; i < points.size(); ++i)
    {
        for (int j = i+1; j < points.size(); ++j)
        {
            int x1 = points[i][0], y1 = points[i][1];
            int x2 = points[j][0], y2 = points[j][1];
            if (x1 == x2 || y1 == y2) continue;
            if (rec[x1].find(y2) != rec[x1].end() && rec[x2].find(y1) != rec[x2].end())
                res = min(res, abs(x1-x2) * abs(y1-y2));
        }
    }
    return res != INT_MAX ? res : 0;
}
```

### [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
```c++
// Simple approach is to sort and then a linear scan will do - O(nlogn)

// We can use DSU here like this, complexity will be O(N * α(n)) where α is inverse ackermann function which
// on avg is O(1) but in worst case O(logn)
unordered_map<int, int> parent, sz;
int findSet(int x) { return (x == parent[x]) ? x : parent[x] = findSet(parent[x]); }
void unionSet(int x, int y)
{
    x = findSet(x), y = findSet(y);
    if (x != y)
    {
        if (sz[x] < sz[y]) swap(x, y);
        parent[y] = x;
        sz[x] += sz[y];
    }
}
int longestConsecutive(vector<int>& nums)
{
    unordered_set<int> st;
    parent.clear(), sz.clear();
    for (const int x : nums)
    {
        st.insert(x);
        parent[x] = x;
        sz[x] = 1;
    }

    for (const int x : nums)
    {
        if (st.find(x-1) != st.end())
            unionSet(x, x-1);
    }

    int res = 0;
    for (const int x : nums) res = max(res, sz[x]);
    return res;
}

// O(N) solution, it may look like N^2 but it's actually linear
// because if condition inside the for loop is called only when x marks the beginning of sequence
// hence still making the complexity O(N + N) instead of O(N * N)
int longestConsecutive(vector<int>& nums)
{
    set<int> st;
    for (const int x : nums) st.insert(x);

    int res = 0;
    for (const int x : st)
    {
        if (st.find(x-1) == st.end())
        {
            int curNum = x, curLen = 1;
            while (st.find(curNum+1) != st.end())
                curNum++, curLen++;
            res = max(res, curLen);
        }
    }
    return res;
}
```

### [Rotate Image](https://leetcode.com/problems/rotate-image/)

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix)
    {
        int n = matrix.size();
        for (int i = 0; i < n/2; ++i)
        {
            for (int j = i; j < n-1-i; ++j)
            {
                int a = matrix[i][j];
                int b = matrix[j][n-1-i];
                int c = matrix[n-1-i][n-1-j];
                int d = matrix[n-1-j][i];

                matrix[i][j] = d;
                matrix[j][n-1-i] = a;
                matrix[n-1-i][n-1-j] = b;
                matrix[n-1-j][i] = c;
            }
        }
    }
};
```

### [Max Sum of Rectangle No Larger Than K](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/)

```cpp
int maxSumSubmatrix(vector<vector<int>>& matrix, int k)
{
    if (matrix.empty()) return 0;
    int n = matrix.size(), m = matrix[0].size(), res = INT_MIN;
    for (int l = 0; l < m; ++l)
    {
        vector<int> sm(n, 0);
        for (int r = l; r < m; ++r)
        {
            for (int i = 0; i < n; ++i) sm[i] += matrix[i][r];
            set<int> st;
            st.insert(0);
            int curSm = 0, curMx = INT_MIN;
            for (const int x : sm)
            {
                curSm += x;
                auto it = st.lower_bound(curSm-k);
                if (it != st.end()) curMx = max(curMx, curSm - *it);
                st.insert(curSm);
            }
            res = max(res, curMx);
        }
    }
    return res;
}
```

## Extras

* [BerSU Ball](https://codeforces.com/problemset/problem/489/B) - Given two arrays of n & m length, we have to find pairs between a & b array such that a\[i\] & a\[j\] differs by atmost 1. **Solution:** Sort both arrays and then initialise pointers. If a\[i\] & b\[j\] form a pair then increment both i & j otherwise if a\[i\] &lt; b\[j\] then increment i else increment j. [https://codeforces.com/contest/489/submission/70360917](https://codeforces.com/contest/489/submission/70360917)
* [Random Teams](https://codeforces.com/problemset/problem/478/B) - Given n & m. n peoples form m teams \(can be uneven\) each group members become friends. We need to find minimum & maximum number of friends possible. **Solution:** If we distribute evenly n/m, n/m ... m then it will give minimum number of friends while if we divide like 1 1 ... n-m+1 this will give maximum. Now if there are n peoples then n\*\(n-1\)/2 will be friends pair count. [https://codeforces.com/contest/478/submission/70420746](https://codeforces.com/contest/478/submission/70420746)
* [A and B and Team Training](https://codeforces.com/problemset/problem/519/C) - Given n & m, n is number of experienced peoples & m are newbies. A team can be exp-new-new or exp-exp-new. We have to tell max number of teams possible. **Solution:** Super easy simulation [https://codeforces.com/contest/519/submission/70423733](https://codeforces.com/contest/519/submission/70423733)
* [Anton and currency you all know ](https://codeforces.com/problemset/problem/508/B)- Given a number, swap its element to make max possible even number. **Solution:** If given number is 44443 ans will be 44434 while if it's 22227 -&gt; 72222. So logic is iterate from left and find first even number which is less then last digit. Also iterate from right and find first even number which is greater than last digit. Print max of both two. [https://codeforces.com/contest/508/submission/70432949](https://codeforces.com/contest/508/submission/70432949)
* [Soldier and Badges](https://codeforces.com/problemset/problem/546/B) - Given an array, with possibly duplicate values. We can increment an element by 1 with cost of 1. We need to find minimum cost such that every element become distinct. [https://codeforces.com/contest/546/submission/70433637](https://codeforces.com/contest/546/submission/70433637)
* [Product of Three Numbers](https://codeforces.com/problemset/problem/1294/C) - Given a number we need to find if it's possible to represent it as product of three distinct numbers such that each is atleast 2. If yes then print numbers aswell. **Solution:** Find prime factors of number and then iterate it while maintaining duplicacy property [https://codeforces.com/contest/1294/submission/70434092](https://codeforces.com/contest/1294/submission/70434092)
* [Building Permutation](https://codeforces.com/problemset/problem/285/C) - Given an array we need to convert it into a permutation \(a permutation is an array of size n with distinct elements ranging 1..n\) we can either decrease or increase the element in one move. Find min moves. **Solution:** Sort the elements and then try matching ith element with \(i+1\)th number hence forming permutation [https://codeforces.com/contest/285/submission/70436700](https://codeforces.com/contest/285/submission/70436700)
* [Simple Game](https://codeforces.com/problemset/problem/570/B) - Two people are playing a game where they pick any number from 1 to n and then a random number is generated whosoever number absolute closest to random number wins, in case of draw Misha wins. First Misha chooses m. Now you Andrew has to choose such that probability of winning is maximized. Find number Andrew must choose. **Solution:** The random number c can be anywhere so we must think of a place that makes it most of the time closest. Middle element is always neutral to pick but say if Misha chose a number from beginning \(1.2..\) then we would want to pick m+1. Same if she choose end elements we would want to pick m-1. [https://codeforces.com/contest/570/submission/70438405](https://codeforces.com/contest/570/submission/70438405)
* [Interesting Subarray](https://codeforces.com/problemset/problem/1270/B) - Given t testcases, in each an array of n size. We have to find a subarray such that max of that subarray - min of it &gt;= n. We can print any such subarray or if not possible return NO. **Solution:** For a valid such substring of size x if we keep reducing it's size we eventually reach to a two element subarray which is also valid. So the answer is to keep iterating considering 2 elements and finding if those two max - min &gt;= 2 If yes ans is true [https://codeforces.com/contest/1270/submission/70494617](https://codeforces.com/contest/1270/submission/70494617)
* [Equalize](https://codeforces.com/problemset/problem/1037/C) - Given two binary string a & b. We want to convert a to b. We can either choose two points i & j within a and flip both i and j bits it will take abs\(i-j\) cost or we can flip any bit individually it will take 1 cost. **Solution:** We want to swap ith bit which is turned on in a but not in b to the nearest turned on bit in b. We can use bubble sort like approach here because it's easier to implement. For rest we can simply use 2nd operation. [https://codeforces.com/problemset/problem/1037/C](https://codeforces.com/problemset/problem/1037/C)
* [Block Adventure](https://codeforces.com/problemset/problem/1200/B) - Given t testcases, in each 3 number n, m, k. n is number of block platforms currently player is on 1st block platform. m are number of additional blocks player has he can add or remove blocks from the platform he's currently at and finally k is threshold by which he can jump so if gap between two concecutive block platforms is atmost k he can jump. Next line also contains heights of all n blocks. We need to find out if it's possible to reach the end. **Solution:** Greedy strategy will be to always lower down the block platform height uptil threshold point. [https://codeforces.com/contest/1200/submission/70517364](https://codeforces.com/contest/1200/submission/70517364)
* [Kill 'Em All](https://codeforces.com/problemset/problem/1238/B) - Given t testcases in each two numbers n and r. Next line we have position of n monsters in a 1D line. We can throw a bomb at any point after explosion all the monsters to the left of bomb will go -r and right one +r. Those monsters who are exactly at that point will die. We need to find minimum number of bombs we can throw. **Solution:** Strategy is to always throw bomb to the rightmost point \(remove duplicates\). [https://codeforces.com/contest/1238/submission/70556569](https://codeforces.com/contest/1238/submission/70556569)
* [Prefix Sum Primes](https://codeforces.com/problemset/problem/1150/C) - Given an array of size n containing only 2s & 1s we have to rearrange it such that it's prefix sum will give maximum primes. So \[1 2 1 2 1\] -&gt; \[1 1 1 2 2\] -&gt; prefix\[1 2 3 5 7\] 4 of them are primes. [https://codeforces.com/contest/1150/submission/70518072](https://codeforces.com/contest/1150/submission/70518072)
* [Longe Number](https://codeforces.com/problemset/problem/1157/B) - Given a number string of size n and in next line 9 numbers corresponding to f\[1\] = ? f\[2\] = ? ... and so on meaning that we can swap 1st number with that number instead. We have to choose a subsegment and swap all it's elements according to that mapping function given such that resulting number is maximum. [https://codeforces.com/contest/1157/submission/70560147](https://codeforces.com/contest/1157/submission/70560147)
* [Good Numbers](https://codeforces.com/problemset/problem/1249/C1) - A good number is one with sum of distinct power of 3s. You are given t testcases within each a number n find the smallest number greater than or equal to n. [Hard Version](https://codeforces.com/problemset/problem/1249/C2) **Solution:** Idea is to create a mod 3 number. If it's good then there must be no 2s in it. To convert it into nearest good number add +1 to digit wherever there's 2 and keep adding carry. [https://codeforces.com/contest/1249/submission/70586120](https://codeforces.com/contest/1249/submission/70586120)
* [Nice Garlands](https://codeforces.com/problemset/problem/1108/C) - Given a string of n length containing R G B. We have to recolor minimum elements such that all elements which are same have abs\(i-j\) % 3 = 0. **Solution:** There are 6 possibilities - RGB RBG BGR BRG GBR GRB our string must contain repeated such elements in order to be good. [https://codeforces.com/contest/1108/submission/70593098](https://codeforces.com/contest/1108/submission/70593098)
* [Array Sharpening](https://codeforces.com/problemset/problem/1291/B) - Given an array we can decrease any of it's element by 1 we want to make it sharpened i.e. it must have a strictly increasing peak at some point. a1ak+1&gt;…&gt;an **Solution:** Consider a peaked graph everything above it or equal to it must give ans as YES. For even elements in graph there will be two such peak graphs. [https://codeforces.com/contest/1291/submission/70754281](https://codeforces.com/contest/1291/submission/70754281)
* [Reach Median](https://codeforces.com/problemset/problem/1037/B) - Given an array of n length which is odd. We have to make median of it exactly s. We can increase or decrease a number by 1 [https://codeforces.com/contest/1037/submission/70594011](https://codeforces.com/contest/1037/submission/70594011)
* [Sanatorium](https://codeforces.com/problemset/problem/732/C) - You went on a hotel and stayed. You had b breakfast, d dinner and s suppers. In morning breakfast then dinner then supper. You don't remember when you morning evening or night and when you left. You could also miss some meals in between. Find the minimum number of meals you might have skipped. [https://codeforces.com/contest/732/submission/70721905](https://codeforces.com/contest/732/submission/70721905)
* [Accordion](https://codeforces.com/problemset/problem/1101/B) - An accordian is a string \[:\|\|\|\|:\] this '\|' can appear any number of times possibly zero. We have to transform a string to accordion by deleting minimum of it's character. [https://codeforces.com/contest/1101/submission/70749874](https://codeforces.com/contest/1101/submission/70749874)
* [Good String](https://codeforces.com/problemset/problem/1165/C) - Given a string, it's of even length and every element at odd position is not equal to it's next element. We have to convert a given string to good by removing minimum elements. [https://codeforces.com/problemset/problem/1165/C](https://codeforces.com/problemset/problem/1165/C)
* [Increasing Subsequence](https://codeforces.com/problemset/problem/1157/C1) - Given an array, we can pick numbers from both of it's end after picking we have to maintain an increasing subsequence. Hard version can have same numbers [https://codeforces.com/contest/1157/submission/70756205](https://codeforces.com/contest/1157/submission/70756205) \[EASY\] [https://codeforces.com/contest/1157/submission/70757245](https://codeforces.com/contest/1157/submission/70757245) \[HARD\]
* [Pride](https://codeforces.com/problemset/problem/891/A) - Given an array of size n. If two adjacent elements have gcd g then we can replace anyone of those adjacent element with g in 1 operation. We want to convert entire array to 1s in min operations. **Solution:** First observation is if the array has atleast one 1 then n-\(ones\) will be solution. If not we will have to make atleast one 1 and after that n-\(ones\) logic will be applicable. For that we are looking for all N^2 subarrays finding gcd of all of them and overall finding minimum subsegment having gcd 1. [https://codeforces.com/contest/891/submission/72689850](https://codeforces.com/contest/891/submission/72689850)
* [Party Lemonade](https://codeforces.com/problemset/problem/913/C) - Given n lemonades, ith have 2^i volume \(0 indexing\) and costs c\[i\] \(Given cost c array\) also given a number l. You have to atleast have l volume of lemonade in minimum amount. **Solution:** First scenerio that we must consider is this \[20 30 70 90\] and l = 12. We should first make an optimal array \[20 30 60 90\] here i denotes optimal cost of having 2^i volume. Now we just have to greedily pick from right end. [https://codeforces.com/contest/913/submission/72953693](https://codeforces.com/contest/913/submission/72953693)
* [K For The Price of One](https://codeforces.com/problemset/problem/1282/B2) - Given k items of given cost \(given cost array\) also given we have q cash and there's an offer by which we can pick k items for the price of max\(chosen k items\) we want to maximize items purchased. \(In easy version k = 2\) **Solution:** Simple greedy sort the costs \[1 2 3 9 10\] k = 3 Then make a dp which dp\[i\] indicates min cost required to buy items till i. \[1 3 3 10 13\] [https://codeforces.com/contest/1282/submission/72959571](https://codeforces.com/contest/1282/submission/72959571)
* [Alternative Thinking](https://codeforces.com/problemset/problem/603/A) - Given a binary string on n length we can flip a subsegment out of it in order to maximize alternating subsequence i.e. 0 then 1 then 0... **Solution:** It's a simple dp problem to calculate longest subsegment, before finding that apply a greedy strategy to start flipping when we encounter same bit consecutively until the end. [https://codeforces.com/contest/603/submission/72984127](https://codeforces.com/contest/603/submission/72984127)
* [Standard Free2Play](https://codeforces.com/problemset/problem/1238/C) - You are at height h and there are pillars at all heights however only n of them are in front of you \(given as array\). With a device we can toggle pillar of height x and x-1. There might be a deadlock at some case we can magically change any pillar to hidden or revealed at 1 cost. We have to minimize cost and reach 0 height. Player can land x-2 but not x-3. **Solution:** Say 5 4 3 2 /1/ 0 is the case where /1/ is hidden. Other case is 4 3 2 /1/ 0. Solution is to think if we have continuous blocks then we can switch 5 and 4 still land safely to 3 but in 1st example since 1 is hidden second time it won't be able to land. So if count is even then it won't be able to land so ans++. [https://codeforces.com/contest/1238/submission/73351853](https://codeforces.com/contest/1238/submission/73351853)
* [Color Stripe](https://codeforces.com/problemset/problem/219/C) - Given a string of n length containing capital letters indicating that cell is colored with Ath color. There can be k colors. We have to recolor the string such that finally no adjacent cells are colored same. Solution: Consider 2 cases, if k &gt;= 3 then we just have to greedily pick current cell since only atmost 2 neighbours matters to us but in case k = 2 greedily picking might not give optimal result but we can think there can be only two string possibility here ABABAB or BABABA check both for k=2 see which one is optimal. Again for K &gt;= 3 greedily check is str\[i\] == str\[i-1\] then assign char such that it doesn't coincide with both back and front. Note that if \(str\[i\] == str\[i-1\] \|\| str\[i\] == str\[i+1\) can give wrong answer. [https://codeforces.com/contest/219/submission/73408098](https://codeforces.com/contest/219/submission/73408098)
* [Ayoub's Function](https://codeforces.com/problemset/problem/1301/C?locale=en) - Given t testcases in it 2 integers n & m we have to create a binary string containing only 0 & 1 of n length containing m zeroes such that no. of substrings having atleast 1 zero is maximized. **Solution**: Rightaway if we think greedy we have to form binary string with maximum possible symmetry that will give the optimal answer. And after arranging 1's in it we want to calculate all possible such substrings which is N^2 and since constraints are high we can't do it like this. We can break this problem to all_substrings - substrings\_with\_all\_zeroes number of substrings are \(n\*\(n+1\)\)/2 to calculate substring with all zeroes we have to think about the placing we did before. \_1\_1\_1_ After placing all the 1's \(m\) we will left with \(m+1\) buckets in which we can put 0's. We can leve a bucket empty aswell but we want to fill it uniformly in order to get optimal result. We have \(n-m\) zeroes divided in \(m+1\) buckets: quo = \(n-m\)/\(m+1\) rem = \(n-m\)%\(m+1\) So all buckets will have atleast quo amount of zeroes in it while rem buckets out of them will have quo+1 amount of zeroes [https://codeforces.com/contest/1301/submission/71083396](https://codeforces.com/contest/1301/submission/71083396)
* [Sasha and a Bit of Relax](https://codeforces.com/problemset/problem/1109/A) - Given an array of size n we want to find how many contiguous subarrays are there such that a\[l\] ^ a\[l+1\] ... a\[mid\] = a\[mid+1\] ^ a\[mid+2\] ... a\[r\]. This property must hold and r-l+1 should be even i.e. lhs and rhs have equal number of elements. **Solution:** solving a\[l\] ^ a\[l+1\] ... a\[mid\] = a\[mid+1\] ^ a\[mid+2\] ... a\[r\] we will get a\[l\] ^ a\[l+1\] ... a\[r\] = 0. So basically we want to count subsegment with even length and xor = 0. Create a prefix xor to do it in O\(N\). In prefix xor of i & j element means xor from ith element to jth in original array. So now we just want to find pair in xor prefix with even length and xor = 0. [https://codeforces.com/contest/1109/submission/73284691](https://codeforces.com/contest/1109/submission/73284691)
* [Hard Process](https://codeforces.com/problemset/problem/660/C) - Given a binary array of size n we can perform atmost k flips we want to maximize contiguous 1s or 0s. **Solution:** Use adjusting window technique in order to calculate optimal range like this. [https://codeforces.com/contest/660/submission/73364852](https://codeforces.com/contest/660/submission/73364852)
* [Berry Jam](https://codeforces.com/problemset/problem/1278/C) - Given a 2\*n array having 2 types of jar. Right in middle player is standing and he wants to eat from middle \(minimum jar from both end\) to balance out jar types. **Solution:** Consider prefix array and start from n-1 to 2\*n. For simplicity we can consider jar 1 & -1 so prefix array will be much easier. [https://codeforces.com/contest/1278/submission/73368897](https://codeforces.com/contest/1278/submission/73368897)
* [Anton and Making Potions](https://codeforces.com/contest/734/problem/C) - Given n, m, k you have to create n potions given m spells of type1 and k spells of type 2. Also given x, s. x is initial time to create 1 spell. s is total mana we have. Next given 2 vectors of size m & k representing type1 and type2 spell. Each vector is pair having mana point it requires and \(type1: new rate that it will give\) \(type2: potions that it will instantly create\). Find minimum time we can have to create n potions. **Solution:** Simple 2 pointer sorting problem. [https://codeforces.com/contest/734/submission/73401040](https://codeforces.com/contest/734/submission/73401040)
* [Booking System](https://codeforces.com/problemset/problem/416/C) - Given n groups of people having bookings, each group pays x amt and have y members in it. You have m tables each having distinct capacity. Each table must store only 1 group. Find which all group to take and which to not. **Solution:** It's basic greedy, start from picking those group which pays maximum try to fit them in smallest table possible. [https://codeforces.com/contest/416/submission/73403170](https://codeforces.com/contest/416/submission/73403170)
* [Sagheer, the Hausmeister](https://codeforces.com/problemset/problem/812/B) - Given n floors and m rooms \(0th is left stair m+1th is right stair in between m rooms\) also given bitset representing rooms with light turned on. Initially you are at ground floor left side you want to turn all lights off in minimum time. It consumes 1 unit time to move from any cell to neighbouring one. You cannot go up without turning all lights on same floor. **Solution:** Simple recursive no memoization required [https://codeforces.com/contest/812/submission/73486463](https://codeforces.com/contest/812/submission/73486463)
* [Maximum XOR Secondary](https://codeforces.com/contest/281/problem/D) - You’ve been given a sequence of distinct positive integers. Your task is to find a interval \[l, r\], so that the maximum element xor the second maximum element within the interval is as large as possible. **Solution:** The interval’s meaning can only be reflected on its maximum element and second maximum element, so apparently, there must be a lot of meaningless interval which we needn’t check them at all. But how can we skip them? Maintain a monotone-decreasing-stack can help us. While a new element came into the view, pop the top element in the stack, and check the corresponding interval, until the new element is greater than the top element in the stack. We can easily see it is correct since we won’t lost the answer as long as it exists. [https://codeforces.com/contest/281/submission/74735528](https://codeforces.com/contest/281/submission/74735528)
* [Counting Kangaroos is Fun](https://codeforces.com/contest/372/problem/A) - Given an array we want to find pairs such that a\[i\] &lt;= a\[j\] group those pair and in the end return count of groups. **Solution:** [https://codeforces.com/contest/448/submission/74697633](https://codeforces.com/contest/448/submission/74697633)
* [Single-use Stones](https://codeforces.com/contest/965/problem/D) - [https://codeforces.com/contest/965/submission/76662788](https://codeforces.com/contest/965/submission/76662788)
* [Nauuo and Cards](https://codeforces.com/contest/1173/problem/C) - Observe first two testcases and try to come up with a greedy strategy in 1st we are directly pushing and never considering empty card so first try that strategy next consider picking empty cards so for all i 1 to n: pos\[i\]-\(i-1\) will denote how much empty card we will require to put. If we play these many blank cards so the top of pile \(hand\) above this position are already in our hand. So the ans is max\(pi — i + 1\) + n. We add n for playing the cards 1 to n. [https://codeforces.com/contest/1173/submission/76922325](https://codeforces.com/contest/1173/submission/76922325)
* [Two Small Strings](https://codeforces.com/contest/1213/problem/E) - We can do something like stress test since strings are small so generate all possible permutations and finally check if string matches constraint. [https://codeforces.com/contest/1213/submission/78345167](https://codeforces.com/contest/1213/submission/78345167)

