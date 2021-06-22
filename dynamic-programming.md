# Dynamic Programming

## Classical

### Friends Pairing Problem

```text
Given n friends we can pair them in the end find total ways
Input  : n = 3
Output : 4

Explanation
{1}, {2}, {3} : all single
{1}, {2, 3} : 2 and 3 paired but 1 is single.
{1, 2}, {3} : 1 and 2 are paired but 3 is single.
{1, 3}, {2} : 1 and 3 are paired but 2 is single.
Note that {1, 2} and {2, 1} are considered same.

Answer:
dp[i] = dp[i-1] + (n-1) * dp[n-2]
nth person can remain single so dp[n-1] + he can be coupled with other (n-1)
person making dp[n-2] * (n-1)
```

### Jump Game

* [https://leetcode.com/problems/jump-game/](https://leetcode.com/problems/jump-game/)
* [https://leetcode.com/problems/jump-game-ii/](https://leetcode.com/problems/jump-game-ii/)

```cpp
bool canJump(vector<int>& nums)
{
    int lastPos = nums.size()-1;
    for (int i = nums.size()-2; i >= 0; --i)
        if (i+nums[i] >= lastPos) lastPos = i;
    return lastPos == 0;
}
// TLE
int jump(vector<int>& A)
{
    int n = A.size();
    vector<long long> dp(n, INT_MAX);
    dp[n-1] = 0;
    for (int i = n-2; i >= 0; --i)
    {
        for (int j = i+1; j <= min(n-1, i+A[i]); ++j)
            dp[i] = min(dp[i], 1 + dp[j]);
    }
    return dp[0];
}

/* Simpler solution is with BFS. Start from index 0 and keep
moving, Why BFS? because we want to find shortest jump so BFS will
go in layer wise manner hence will find solution quickest. */
int jump(vector<int>& nums)
{
    int n = nums.size();
    queue<int> q;
    vector<bool> visited(n, false);
    q.push(0);
    visited[0] = true;
    int res = 0;
    while (!q.empty())
    {
        int sz = q.size();
        while (sz--)
        {
            int cur = q.front();
            q.pop();
            if (cur == n-1) return res;
            for (int i = cur+1; i <= min(n-1, cur+nums[cur]); ++i)
            {
                if (visited[i]) continue;
                visited[i] = true;
                q.push(i);
            }
        }
        res++;
    }
    return -1;
}
/* This above solution however gives TLE, even though we are
visiting every node ones but there we are checking through for loop
to avoid this we can extend above idea in a range wise BFS

[2, 3, 1, 1, 4]
Initially our queue holds 0 element so l = 0, r = 0
then we get 1st and 2nd element in our queue making l = 1, r = 2
then we get l = 3, r = 4
Basically just extending the idea in range manner */
int jump(vector<int>& nums)
{
    int n = nums.size(), l = 0, r = 0, res = 0;
    while (r < n-1)
    {
        int nxt = r;
        for (int i = l; i <= r; ++i) nxt = max(nxt, nums[i]+i);
        l = r+1, r = nxt;
        ++res;
    }
    return res;
}
```

### [Paint Fence](https://www.lintcode.com/problem/paint-fence/description)

```cpp
/* Basically 0011 1100 is allowed 0001 1110 is not allowed.
dp[i] = (k-1) * (dp[i-1] + dp[i-2])
Basically we are taking dp[i-1] combinations and placing a different (non
matching) color to ith one And taking dp[i-2] combinations and painting i-1
th and ith as same color. */
int numWays(int n, int k)
{
    int dp[n+1];
    dp[1] = k, dp[2] = k*k;
    for (int i = 3; i <= n; ++i) dp[i] = (k-1) * (dp[i-1] + dp[i-2]);
    return dp[n];
}
```

### Ugly Number

```cpp
/* An ugly number is the one whose prime factors only include 2, 3, 5.
Find nth ugly number.
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of
the first 10 ugly numbers. */
int nthUglyNumber(int n)
{
    int dp[n];
    dp[0] = 1;
    int x = 0, y = 0, z = 0;
    for (int i = 1; i < n; ++i)
    {
        dp[i] = min({2*dp[x], 3*dp[y], 5*dp[z]});
        if (dp[i] == 2*dp[x]) x++;
        if (dp[i] == 3*dp[y]) y++;
        if (dp[i] == 5*dp[z]) z++;
    }
    return dp[n-1];
}
/* Genric variation with primes set instead of 2, 3, 5 */
int nthSuperUglyNumber(int n, vector<int>& primes)
{
    vector<int> mark(primes.size(), 0);
    int dp[n];
    dp[0] = 1;
    for (int i = 1; i < n; ++i)
    {
        dp[i] = INT_MAX;
        for (int j = 0; j < primes.size(); ++j)
            dp[i] = min(dp[i], primes[j] * dp[mark[j]]);
        for (int j = 0; j < primes.size(); ++j)
            if (primes[j] * dp[mark[j]] == dp[i]) mark[j]++;
    }
    return dp[n-1];
}

/* Now a number is ugly if it's divisible by a, b, c find nth ugly number.
Binary search over values and then count. */
#define ll long long
ll nthUglyNumber(ll n, ll a, ll b, ll c)
{
    ll l = a, r = 1e18;
    ll x1 = lcm(a, b), x2 = lcm(b, c), x3 = lcm(c, a), x4 = lcm(a, lcm(b, c));
    while (l < r)
    {
        ll mid = l + (r-l)/2;
        ll val = mid/a + mid/b + mid/c - mid/x1 - mid/x2 - mid/x3 + mid/x4 + 1;
        if (val > n) r = mid;
        else l = mid+1;
    }
    return l;
}
```

### Decode Ways

* [https://leetcode.com/problems/decode-ways/](https://leetcode.com/problems/decode-ways/)
* [https://leetcode.com/problems/decode-ways-ii/](https://leetcode.com/problems/decode-ways-ii/)

```cpp
// Top Down
vector<int> dp;
int solve(string &str, int cur = 0)
{
    if (cur == str.size()) return 1;
    if (dp[cur] != -1) return dp[cur];
    
    int res = 0;
    if (str[cur] > '0' && str[cur] <= '9')
    {
        res += solve(str, cur+1);
        if (cur+1 < str.size() && (str[cur] < '2' || (str[cur] == '2' && str[cur+1] <= '6')))
            res += solve(str, cur+2);
    }
    return dp[cur] = res;
}
int numDecodings(string s)
{
    dp = vector<int>(s.size(), -1);
    return solve(s);
}

// Bottom Up
int numDecodings(string s)
{
    int n = s.size();
    if (n == 0 || s[0] == '0') return 0;
    int dp[n+1];
    dp[0] = 1, dp[1] = 1;
    for (int i = 2; i <= n; ++i)
    {
        int num = stoi(s.substr(i-2, 2));
        dp[i] = ((num <= 26 && num > 9) ? dp[i-2] : 0) + ((s[i-1] == '0') ? 0 : dp[i-1]);
    }
    return dp[n];
}

// Variant - II
// Bottom up
const int M = 1e9 + 7;
int numDecodings(string s)
{
    int n = s.length();
    vector<long> dp(n+1);
    dp[0] = 1;
    if (s[0] == '0') return 0;
    dp[1] = (s[0] == '*') ? 9 : 1;
    for (int i = 2; i <= n; ++i)
    {
        if (s[i-1] == '0')
        {
            if (s[i-2] == '1' || s[i-2] == '2') dp[i] = dp[i-2];
            else if (s[i-2] == '*') dp[i] = 2*dp[i-2];
            else return 0;
        }
        else if (s[i-1] >= '1' && s[i-1] <= '9')
        {
            dp[i] = dp[i-1];
            if (s[i-2] == '1' || (s[i-2] == '2' && s[i-1] <= '6'))
                dp[i] = (dp[i] + dp[i-2]) % M;
            else if (s[i-2] == '*')
                dp[i] = (s[i-1] <= '6') ? (dp[i] + dp[i-2]*2) % M : (dp[i] + dp[i-2]) % M;
        }
        else
        {
            dp[i] = (dp[i-1]*9) % M;
            if (s[i-2] == '1') dp[i] = (dp[i] + 9*dp[i-2]) % M;
            else if (s[i-2] == '2') dp[i] = (dp[i] + 6*dp[i-2]) % M;
            else if (s[i-2] == '*') dp[i] = (dp[i] + 15*dp[i-2]) % M;
        }
    }
    return dp[n];
}
```

### Minimum Cost Path

![](.gitbook/assets/image%20%28130%29.png)

Right side given matrix we need to go from top left to bottom right we can only go bottom and right otherwise this question would have infinite possibilities. Left side is the dp each cell having minimum cost till that block. min\(dp\[i-1\]\[j\], dp\[i\]\[j-1\]\) + mat\[i\]\[j\]

#### **Another Varient**

![](.gitbook/assets/image%20%2817%29.png)

If we were given condition to move in any 4 direction then there would have been infinite possibilities.

This problem can be solved using combinatorics, consider 7x3 grid we have to travel total 8 steps in which 6 are left and 2 are bottom. So ans is 8C2 or 8C2. Ans is m+n-2 C m-1

### Increasing Path In a Matrix

* [https://leetcode.com/problems/longest-increasing-path-in-a-matrix/](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

```cpp
class Solution {
public:
    vector<int> dx = {+1, 0, -1, 0}, dy = {0, +1, 0, -1};
    int n, m;
    vector<vector<int>> dp;
    int solve(vector<vector<int>> &arr, int x, int y)
    {
        if (dp[x][y] != -1) return dp[x][y];
        int res = 1;
        for (int i = 0; i < 4; ++i)
        {
            int _x = x+dx[i], _y = y+dy[i];
            if (_x >= 0 && _x < n && _y >= 0 && _y < m && arr[_x][_y] > arr[x][y])
                res = max(res, 1 + solve(arr, _x, _y));
        }
        return dp[x][y] = res;
    }
    int longestIncreasingPath(vector<vector<int>>& arr)
    {
        n = arr.size();
        if (n == 0) return 0;
        m = arr[0].size();
        if (m == 0) return 0;
        
        dp.assign(n, vector<int> (m, -1));
        
        int res = 1;
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                res = max(res, solve(arr, i, j));
        return res;
    }
};
```

### Robot in Maze

Given M X N board with robot at bottom left side and he has to reach bottom right. He can go - right, top right, bottom right. Find no. of ways to reach in O\(NM\) time and O\(m\) space

```cpp
/* DP Approach:
0    0    0    1
0    0    1    3    
0    1    2    5
1    1    2    4 */

/* Combinatorics Approach:
we have 3 operations: A = x+1, y    B = x+1, y-1    C = x+1, y+1
Ap + Bq + Cr = dx, dy
p(x+1) + q(x+1) + r(x+1) = dx
(p + q + r)(1 + x) = dx
p + q + r = dx

p(y) + q(y-1) + r(y+1) = dy
y(p + q + r) + (r - q) = dy
dy - dx = r - q
simply use extended eucledian find all possible solution */
```

### Probability to reach a point in a grid

Given W x H grid where L, U, D, R denotes \(left, up, down, right\) part which has been cut out and if player goes there he will fall off. Find the probability of player not falling.

From above discussions we can say P \(Probability of going x, y\) is

$$
P(x, y) = {x+y-2 \choose x-1} / {2^{x+y-2}}
$$

We can precompute nCk/2^n using by taking log both sides

$$
\left(\frac{n \choose k}{2^n}\right) = 2^{log{{n \choose k}} - log{2^n}} = 2^{log{n!} - log{(n-k)!} - log{k!} - n}
$$

![We need to find probToGo to bottomleft of hole and top right of hole then continue DP](.gitbook/assets/image%20%2870%29.png)

```cpp
const int MAXN = 1e5+5;
double logFact[MAXN];
inline double func(int n, int k) { return pow(2, logFact[n] - logFact[n-k] - logFact[k] - n); }
inline double probToReach(int x, int y) { return func(x+y-2, x-1); }
logFact[0] = 0;
for (int i = 1; i < MAXN; ++i) logFact[i] = logFact[i-1] + log2(i);
```

### Dungeon Game

```cpp
/*  each block reduces (say -2) health or increases (say +30) health,
    find health reqd to move from top left to bottom right.
    -2  -3  3           7   5   2
    -5  -10 1           6  11   5
    10  30  -5          1   1   6
DP(Each state represents health reqd from that pos) DP[0][0] is ans */
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon)
    {
        int n = dungeon.size(), m = dungeon[0].size();
        vector< vector<int> > DP(n+1, vector<int>(m+1, INT_MAX));
        DP[n][m-1] = DP[n-1][m] = 1;
        for (int i = n-1; i >= 0; --i)
        {
            for (int j = m-1; j >= 0; --j)
            {
                int reqd = min(DP[i+1][j], DP[i][j+1]) - dungeon[i][j];
                DP[i][j] = reqd <= 0 ? 1 : reqd;
            }
        }
        return DP[0][0];
    }
};
```

### Probability Knight Remain in Chessboard

Given a knight in a chessboard he can move in 8 directions for exactly k steps what is the probability that the knight remains in the chessboard.

```cpp
const int K = 8, N = 8;
double DP[K][N][N];
pair<int, int> moves[] = { {1, 2}, {2, 1}, {2, -1}, {1, -2}, {-1, -2}, {-2, -1}, {-2, 1}, {-1, 2} };
double findProb(int startX, int startY, int steps)
{
    memset(DP, 0, sizeof(DP));
    DP[0][startX][startY] = 1.0;
    for (int k = 1; k <= steps; ++k)
    {
        for (int x = 0; x < N; ++x)
        {
            for (int y = 0; y < N; ++y)
            {
                for (auto &cur : moves)
                {
                    int newX = x + cur.F, newY = y + cur.S;
                    if (newX >= 0 && newY >= 0 && newX < N && newY < N)
                        DP[k][newX][newY] += DP[k-1][x][y] / 8.0;
                }
            }
        }
    }
    double res = 0.0;
    for (int x = 0; x < N; ++x) for (int y = 0; y < N; ++y) res += DP[steps][x][y];
    return res;
}
```

[Markov Chain](https://youtu.be/uvYTGEZQTEs) Solution:

```cpp
/* There'll be 11 degenerate states, Let 10 represent out of board state */
int states[8][8] = {
    { 0,  1,  3,  4,  4,  3,  1,  0 },
    { 1,  2,  5,  6,  6,  5,  2,  1 },
    { 3,  5,  7,  8,  8,  7,  5,  3 },
    { 4,  6,  8,  9,  9,  8,  6,  4 },
    { 4,  6,  8,  9,  9,  8,  6,  4 },
    { 3,  5,  7,  8,  8,  7,  5,  3 },
    { 1,  2,  5,  6,  6,  5,  2,  1 },
    { 0,  1,  3,  4,  4,  3,  1,  0 }
};
/* T[i][]]: Transposition matrix represents probability of going from ith
state to jth */
double findProb(int x, int y, int k)
{
    Matrix<double> T(11, 11);
    T.mat = {
        //0   1   2   3   4   5   6   7   8   9   10
        { 0,  0,  0,  0,  0,  2,  0,  0,  0,  0,  6 },
        { 0,  0,  0,  1,  0,  0,  1,  1,  0,  0,  5 },
        { 0,  0,  0,  0,  2,  0,  0,  0,  2,  0,  4 },
        { 0,  1,  0,  0,  0,  1,  1,  0,  1,  0,  4 },
        { 0,  0,  1,  0,  0,  1,  0,  1,  1,  0,  4 },
        { 1,  0,  0,  1,  1,  0,  1,  0,  1,  1,  2 },
        { 0,  1,  0,  1,  0,  1,  0,  1,  1,  1,  2 },
        { 0,  2,  0,  0,  2,  0,  2,  0,  0,  2,  0 },
        { 0,  0,  1,  1,  1,  1,  1,  0,  2,  1,  0 },
        { 0,  0,  0,  0,  0,  2,  2,  2,  2,  0,  0 },
        { 0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  8 }
    };
    T *= (1 / (double)8);
    return (1.0 - (T^k).mat[states[x][y]][10]);
}
```

### Domino Problems

```cpp
/* You have a 2x1 domino and L shaped tromino. Find ways to fill it in 2xN mat
dp[0][N] represents ways to fill till N col. complete
dp[1][N] represents ways to fill till N col. with last col. having a vacancy */
const int MOD = 1e9 + 7;
int numTilings(int N)
{
    long long dp[2][max(3, N+1)];
    dp[0][1] = 1, dp[0][2] = 2, dp[1][1] = 0, dp[1][2] = 2;
    for (int i = 3; i <= N; ++i)
    {
        dp[0][i] = ((dp[0][i-1] + dp[0][i-2])%MOD + dp[1][i-1])%MOD;
        dp[1][i] = ((2*dp[0][i-2])%MOD + dp[1][i-1]) % MOD;
    }
    return dp[0][N];
}
```

For problems: Given N x M grid find number of ways to place 1 x 2 and 2 x 1, tiles in it there's a formula which works in O\(NM\) Otherwise it's a complex DP problem with O\(n \* 2^m\) having 2^m instead of 2 \(same idea as above solution\).

![](.gitbook/assets/image%20%2825%29.png)

```cpp
#define M_PI 3.14159265358979323846
inline int ceil(int a, int b) { return (a+b-1)/b; }
int solve(int n, int m)
{
    if (n < 2 && m < 2) return 0;
    int ans = 1;
    for (int a = 1; a <= ceil(n, 2); ++a)
		    for (int b = 1; b <= ceil(m, 2); ++b)
			      ans = ans * (4 * ((cos((M_PI*a)/(n+1)) * cos((M_PI*a)/(n+1))) + (cos((M_PI*b)/(m+1)) * cos((M_PI*b)/(m+1)))));
    return ans;
}
```

### Wine Selling Problem

```cpp
//We will use 2D DP storing l & r
int dp[1000][1000];
int solve(int arr[], int l, int r, int year)
{
    if (l > r) return 0;
    if (dp[l][r] != -1) return dp[l][r];
    int q1 = arr[l]*year + solve(arr, l+1, r, year+1);
    int q2 = arr[r]*year + solve(arr, l, r-1, year+1);
    int ans = max(q1, q2);
    dp[l][r] = ans;
    return ans;
}
//Without memoization it's complexity was O(2^n) with memoization it is O(N^2)

//Bottom up approach
int solve(int arr[], int n)
{
    int dp[1000][1000] {};
    int year = n;
    for (int i = 0; i < n; ++i) dp[i][i] = year*arr[i];
    --year;
    for (int len = 2; len <= n; ++len)
    {
        int l = 0, r = n-len;
        while (l <= r)
        {
            int temp = l+len-1;
            dp[l][temp] = max(arr[l]*year + dp[l+1][temp],
                arr[temp]*year + dp[l][temp-1]);
            ++l;
        }
        -year;
    }
    return dp[0][n-1];
}

/* For [2, 3, 5, 1, 4]
    2       3       5       1       4
2   10      23      43
3   0       15      37      40
5   0       0       25      29
1   0       0       0       5       24
4   0       0       0       0       20
*/

// Quick way to do diagonal filling (excludes diagonal)
for (int i = 1; i < n; ++i)
    for (int j = 0, k = i; k < n; ++k, ++j)
```

* Similar: [https://www.geeksforgeeks.org/optimal-strategy-for-a-game-dp-31/](https://www.geeksforgeeks.org/optimal-strategy-for-a-game-dp-31/)

### Matrix Chain Multiplication

```text
Given A (10x30) B (30x5) C (5x60) then we need to find
order of multiplication such that operations are
minimized

(AB)C = (10×30×5) + (10×5×60) = 4500 operations
A(BC) = (30×5×60) + (10×30×60) = 27000 operations.

         0             1                 2
0   [0, 10x30]     [1500, 10x5]      [4500, 10x60]
1                  [0, 30x5]         [9000, 30x60]
2                                    [0, 5x60]
```

### Burst Balloon Problem

```text
Given values of balloons: 3 1 5 8
Each bursting means that value * left * right value
We need to burst them such that max val: like in
order-5 1 3 8 (1*5*8 + 3*1*8 + 3*8 + 8 = 96)

Diagonals represents first balloon which we burst. 0 1 means
balloons within 0 to 1
    0   1   2   3
0   3  30  159  167
1      15  135  159
2          40   48
3               40

For [0, 2] value left [0, 1] denotes what if we choose [0, 1] to
burst first then we will burst 2 so for ever cell it's
dp[i][j] = max(dp[i][j-1] + arr[j]*arr[j+1], dp[i+1][j] +
arr[0]*arr[j+1])
```

### Rod Cutting Problem

```text
We have rod of length 5 with pieces value: 2, 5, 7, 8 for
1, 2, 3 & 4 respectively.
    1   2   3   4   5
2   2   4   6   8   10
5   2   5   7   10
5
7
```

### Longest Increasing Subsequence

```cpp
/* [3, 10, 2, 1, 20] = [3, 10, 20] = 3
[3, 2] = [3] or [2] = 1
[50, 3, 10, 7, 40, 50] = [3, 7, 40, 50] or [3, 10, 40, 50] = 4 */
// N squared solution is very easy to have

/* NlogN approach: Length of st denotes LIS formed by including
currently encountered element and maintaining an analogous LIS
behavior. */
int lengthOfLIS(vector<int>& nums)
{
    set<int> st;
    for (int x : nums)
    {
        auto lb = st.lower_bound(x);
        if (lb != st.end()) st.erase(lb);
        st.insert(x);
    }
    for (auto &x : st) cout << x << " ";
    return st.size();
}

/* If the problem was Find Long Increasing Bitonic subsequence,
bitonic means a subsequence which first increases and then
decreases. Idea is find LIS from Left 2 Right and Right 2 Left
and then add both array then -1.

2 -1  4  3  5 -1  3  2   [Array]
1  1  2  2  3  1  2  2   [L2R LIS]
2  1  3  2  3  1  2  1   [R2L LIS]
2  1  4  3  5  1  3  2   [Longest Increasing bitonic sequence] */

/* Counting minimum number of Increasing subsequences,
[1 3 2 4] it's - 2 i.e 1,3 and 2,4
[4 3 2 1] it's - 4 i.e. 4,3,2,1 */
multiset<int> st;
for (auto &x : arr)
{
    auto cur = st.lower_bound(x);
    if (cur != st.begin()) st.erase(--cur);
    st.insert(x);
}
cout << st.size() << '\n';
```

* [https://leetcode.com/problems/russian-doll-envelopes/](https://leetcode.com/problems/russian-doll-envelopes/)

```cpp
/* Follow up question do we have to take care of orientation? In that case
rotate envelopes initially such first w is small h is large always. */

/* N^2 LIS is easy to impliment, To do it in NlogN we require same LIS logic as before */
int maxEnvelopes(vector<vector<int>>& envelopes)
{
    sort(envelopes.begin(), envelopes.end(), [](auto x, auto y)
    {
        return (x[0] == y[0]) ? (x[1] > y[1]) : (x[0] < y[0]);
    });

    vector<vector<int>> lis;
    auto comp = [](auto x, auto y) { return (x[1] < y[1]) && (x[0] < y[0]); };
    for (const auto x : envelopes)
    {
        auto lb = lower_bound(lis.begin(), lis.end(), x, comp);
        if (lb == lis.end()) lis.push_back(x);
        else *lb = x;
    }
    return lis.size();
}
```

### [Candy](https://leetcode.com/problems/candy)

```cpp
// Naive N^2 approach
int candy(vector<int>& ratings)
{
    int n = ratings.size();
    vector<int> candies(n, 1);
    bool changes = true;
    while (changes)
    {
        changes = false;
        for (int i = 0; i < n; ++i)
        {
            if (i != n-1 && ratings[i] > ratings[i+1] && candies[i] <= candies[i+1])
                candies[i] = candies[i+1]+1, changes = true;
            if (i > 0 && ratings[i] > ratings[i-1] && candies[i] <= candies[i-1])
                candies[i] = candies[i-1]+1, changes = true;
        }
    }
    return accumulate(candies.begin(), candies.end(), 0);
}

// O(N) time with O(N) space approach
int candy(vector<int>& ratings)
{
    int n = ratings.size();
    vector<int> lft(n, 1), rgt(n, 1);
    for (int i = 1; i < n; ++i)
        if (ratings[i] > ratings[i-1]) lft[i] = lft[i-1]+1;
    for (int i= n-2; i >= 0; --i)
        if (ratings[i] > ratings[i+1]) rgt[i] = rgt[i+1]+1;
    int sm = 0;
    for (int i = 0; i < n; ++i) sm += max(lft[i], rgt[i]);
    return sm;
}

/* One pass, constant space appraoch
Observation: Candies are always given increment of 1. Local minimum number of
candies given to a student is 1 so subdistribution is of form 1, 2, 3, ..., n or
n,..., 2, 1 which is (n*(n+1))/2

Ratings [1 3 2 3 2 1]
  *   *
 / \ / \
*   *   *
         \
          *
Give [1 2 1 3 2 1] = 10
End of ith iteration: (from 1)
C U D O N
0 1 0 0 1
0 1 1 1 -1
3 1 0 -1 1
3 1 1 1 -1
3 1 2 -1 -1 */
inline int count(int n) { return (n*(n+1))/2; }
int candy(vector<int>& ratings)
{
    int n = ratings.size();
    if (n <= 1) return n;
    int candies = 0, up = 0, down = 0, oldSlope = 0;
    for (int i = 1; i < n; ++i)
    {
        int newSlope = (ratings[i] < ratings[i-1]) ? -1 : ((ratings[i] == ratings[i-1]) ? 0 : 1);
        if ((oldSlope == 1 && newSlope == 0) || (oldSlope == -1 && newSlope >= 0))
            candies += count(up) + count(down) + max(up, down), up = 0, down = 0;
        if (newSlope == 1) up++;
        else if (newSlope == -1) down++;
        else candies++;
        oldSlope = newSlope;
    }
    candies += count(up) + count(down) + max(up, down) + 1;
    return candies;
}
```

### [Length of Longest Fibonacci Subsequence](https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/)

Find longest subsequences from given strictly increasing array that follows fibonacci property \(x+y = z\)

```cpp
// Method 1 : N^2 log(MAXN_OF_ARR)
class Solution {
public:
    int lenLongestFibSubseq(vector<int>& arr)
    {
        int n = arr.size();
        unordered_set<int> st(arr.begin(), arr.end());
        
        int ans = 0;
        for (int i = 0; i < n; ++i)
        {
            for (int j = i+1; j < n; ++j)
            {
                int x = arr[j], y = arr[i]+arr[j];
                int len = 2;
                while (st.find(y) != st.end())
                {
                    int z = x+y;
                    x = y; y = z;
                    ans = max(ans, ++len);
                }
            }
        }
        return ans >= 3 ? ans : 0;
    }
};

// DP
vector<vector<int>> dp;
int solve(vector<int> &arr, unordered_map<int, int> &rec, int i, int j)
{
    if (dp[i][j] != -1) return dp[i][j];

    int res = 0;
    if (rec.find(arr[i] + arr[j]) != rec.end())
        res = 1+solve(arr, rec, j, rec[arr[i] + arr[j]]);
    return dp[i][j] = res;
}
int lenLongestFibSubseq(vector<int>& arr)
{
    int res = 0;
    unordered_map<int, int> rec;
    dp = vector<vector<int>>(arr.size(), vector<int>(arr.size(), -1));
    for (int i = 0; i < arr.size(); ++i) rec[arr[i]] = i;
    for (int i = 0; i < arr.size(); ++i)
    {
        for (int j = i+1; j < arr.size(); ++j)
        {
            int cur = solve(arr, rec, i, j);
            if (cur) cur += 2;
            res = max(res, cur);
        }
    }
    return res;
}
```

### Longest Common Subsequence (LCS) / Substring

```cpp
/* X = nematode    Y = empty
ans is 3 i.e. emt

              i-->
    ""  A   G   G   T   A   B
""  0   0   0   0   0   0   0
G   0   0   1   1   1   1   1
X   0   0   1   1   1   1   1
T   0   0   1   1   2   2   2
X   0
T   0
A   0
B   0
dp[i][j] = 1+dp[i-1][j-1] or max(dp[i-1][j], dp[i][j-1])
depending upon (x[i] == y[j])

Longest Common Subsequence (in triplet i.e. 3 strings) - O(N^3)*/
for (int i = 0; i < s1.size(); ++i)
{
    for (int j = 0; j < s2.size(); ++j)
    {
        for (int k = 0; k < s3.size(); ++k)
        {
            if (i == 0 || j == 0 || k == 0) dp[i][j][k] = 0;
            else dp[i][j][k] = (s1[i-1] == s2[j-1] && s1[i-1] == s3[k-1])
                    ? dp[i-1][j-1][k-1] + 1
                    : max({dp[i-1][j][k], dp[i][j-1][k], dp[i][j][k-1]});
        }
    }
}
return dp[s1.size()][s2.size()][s3.size()];

/* Longest Common Substring is different say in abcdaf & zbcdf.
bcd is longest common substring whearas bcdf is longest common subsequence.
Now dp[i][j] = 1 + dp[i-1][j-1] if same otherwise 0 */
```

### [Count Distinct Subsequence](https://www.spoj.com/problems/DSUBSEQ/)

```cpp
int n = s.size();
vector<int> dp(n+1, 0);       // subsequence cnt of first i characters
dp[0] = 1;                    // null char has 1 cnt
unordered_map<char, int> last;
for (int i = 1; i <= n; ++i)
{
    // why *2 consider it like a bitmap either we taken element or not
    dp[i] = 2*dp[i-1];
    if (last.find(s[i-1]) != last.end())
        dp[i] = dp[i] - dp[last[s[i-1]]];
    last[s[i-1]] = i-1;
}
/*  ''    a    b    c
    1     2    4    8

    ''    a    a    a
    1     2    3    4 */
```

### Edit Distance

```text
              i-->
    ""  S   A   T   U   R   D   A   Y
""  0   1   2   3   4   5   6   7   8
S   1   0   1   2   3   4   5   6   7
U   2
N   3
D   4
A   5
Y   6

We want to make saturday -> sunday
each cell represents min operations reqd. to convert inp. to out.

At any point this DP represents say for i=3, j=1
We want to make S from SAT this means 2 deletion. so 2 min operations.
replacement: 1 + dp[i-1][j-1]
deletion: 1 + dp[i+1][j]
insertion: 1 + dp[i][j-1]
min of all three is ans of that cell
if charactre matches look for i-1, j-1 element since checking that
element is redundant
```

#### [One Edit Distance](https://www.lintcode.com/en/old/problem/one-edit-distance/)

```cpp
bool isOneEditDistance(string &s, string &t)
{
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

### Distinct Subsequence

```text
Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:

As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)

rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^

    ""  r   a   b   b   b   i   t
 "" 1   1   1   1   1   1   1   1
 r  0   1   1   1   1   1   1   1
 a  0   0   1   1   1   1   1   1
 b  0   0   0   1   2   3   3   3
 b  0
 i  0
 t  0

 if characters match: DP[i-1][j-1] + DP[i][j-1]
 else: DP[i][j-1]
```

### Palindrome Problems

* Longest Palindromic Substring
* Count Palindromic Substring
* Longest Palindromic Subsequence
* Minimum Insertion to form a palindrome
* Palindromic Partition: Minimum cuts to form palindromes
* Count Palindromic Subsequence
* Count Different Palindromic Subsequence

```cpp
// Longest Palindromic Substring O(N^2) and constant space, can be done in O(N) using manacher algorithm
string longestPalindrome(string s)
{
    if (s.size() <= 1) return s;
    int L = 0, R = 0;
    for (int i = 0; i < s.size(); ++i)
    {
        int len = max(expandAroundCenter(s, i, i), expandAroundCenter(s, i, i+1));
        if (len > R-L) L = i - (len-1)/2, R = i + len/2;
    }
    return s.substr(L, R-L+1);
}
int expandAroundCenter(string s, int L, int R)
{
    while (L >= 0 && R < s.size() && s[L] == s[R]) L--, R++;
    return R - L - 1;
}

// Count Palindromic Substring
int countSubstrings(string s)
{
    if (s.size() <= 1) return 1;
    int ans = 0;
    for (int i = 0; i < s.size(); ++i)
        ans += expandAroundCenter(s, i, i) + expandAroundCenter(s, i, i+1);
    return ans;
}

int expandAroundCenter(string &s, int L, int R)
{
    int cnt = 0;
    while (L >= 0 && R < s.size() && s[L] == s[R]) L--, R++, cnt++;
    return cnt;
}

/* Longest Palindromic Subsequence of: agbda
     0    1    2    3    4    5
0    1    1    1    1    3    4
1         1    1    1    3    3
2              1    1    3    3
3                   1    1    1
4                        1    1
5                             1

if s[i] == s[j] then dp[i][j] = dp[i+1][j-1] + 2
else max(dp[i+1][j], dp[i][j-1])


Minimum Insertion to form a palindrome: 
dp[l, r] =  dp[l+1, r-1]                     if first and last char are same
            min(dp[l, r-1], dp[l+1, r])      otherwise

Other solution is: str.length - LCS(str, rev_str).length
LCS here is the maximum possible palindrome.*/

/* Palindromic Partition: Minimum cuts to form palindromes
        abacccbc = 2
    a   b   a   c   c   c   b   c
a   0   1   0   1   2   
b       0   1   2   2   2
a           0   1   1   1   2
c               0   0   0   1   2
c                   0   0   1   1
c                       0   1   0
b                           0   1
c                               0
    if string(i..j) is palindrome DP[i][j] = 0
    else DP[i][j] = 1 + min(DP[i][k] + DP[k+1][j]), k = i to j-1
*/
int minCut(string s)
{
    int n = s.size();
    if (n <= 1) return 0;
    vector< vector<bool> > isPalind(n, vector<bool>(n, false));
    int minCuts[n];
    for (int i = 0; i < n; ++i) minCuts[i] = i;
    for (int j = 0; j < n; ++j)
    {
        for (int i = j; i >= 0; --i)
        {
            if (s[i] == s[j] && ((j-i <= 2) || isPalind[i+1][j-1]))
            {
                isPalind[i][j] = true;
                minCuts[j] = (i == 0) ? 0 : min(minCuts[j], 1 + minCuts[i-1]);
            }
        }
    }
    return minCuts[n-1];
}


/* Count Palindromic Subsequence need not to be distinct:
countPS(str, 0, str.size()-1);
For "bccb" -> b, c, c, cc, b, bb, bcb, bcb, bccb
Complexity: O(N^2)*/
vector<vector<int>> dp;
int solve(string &str, int i, int j)
{
    if (i >= j || j < 0) return 0;
    if (i == j) return 1;
    if (j-i == 1) return (str[i] == str[j]) ? 3 : 2;
    if (dp[i][j] != -1) return dp[i][j];

    if (str[i] == str[j])
        return dp[i][j] = solve(i+1, j) + solve(i, j-1) + 1;
    else
        return dp[i][j] = solve(i+1, j) + solve(i, j-1) - solve(i+1, j-1);
}
int countPS(string &str)
{
    dp = vector<vector<int>>(str.size(), vector<int>(str.size(), -1));
    return solve(str, 0, str.size()-1);
}

/* Count Different Palindromic Subsequence, */
const int MOD = 1e9 + 7;
vector<vector<int>> dp;
int solve(string &s, int i, int j)
{
    if (i > j) return 0;
    if (i == j) return 1;
    if (dp[i][j] != -1) return dp[i][j];
    
    int res = 0;
    if (s[i] == s[j])
    {
        res = (2*solve(s, i+1, j-1)) % MOD;

        int l = i+1, r = j-1;
        while (l <= r && s[l] != s[i]) ++l;
        while (l <= r && s[r] != s[i]) --r;

        if (l < r) res = (res - solve(s, l+1, r-1) + MOD) % MOD;
        else if (l == r) res = (res + 1) % MOD;
        else res = (res + 2) % MOD;
    }
    else res = ((solve(s, i+1, j) + solve(s, i, j-1)) % MOD - solve(s, i+1, j-1) + MOD) % MOD;
    return dp[i][j] = res;
}
int countPalindromicSubsequences(string S)
{
    dp = vector<vector<int>>(S.size(), vector<int>(S.size(), -1));
    return solve(S, 0, S.size()-1);
}

// Longest Palindromic Substring in O(N) using Manacher's Algorithm
string longestPaindromicManacher(string str, int &start, int &end)
{
    int n = str.size();
    if (n == 0) return "";
    if (n == 1) return str;
    n = 2*n + 1;
    int L[n];
    L[0] = 0, L[1] = 1;
    int C = 1, R = 2, i = 0, iMirror, maxLPSLength = 0, maxLPSCenterPosition = 0, diff = -1;
    start = -1, end = -1;
    for (i = 2; i < n; i++)
    {
        iMirror = 2*C-i, L[i] = 0, diff = R - i;
        if(diff > 0) L[i] = min(L[iMirror], diff);
        while (((i + L[i]) < n && (i - L[i]) > 0) && (((i + L[i] + 1) % 2 == 0) ||  
            (str[(i + L[i] + 1)/2] == str[(i - L[i] - 1)/2] )))
        {
            L[i]++;
        }
        if(L[i] > maxLPSLength) maxLPSLength = L[i], maxLPSCenterPosition = i;
        if (i + L[i] > R) C = i, R = i + L[i];
    }
    start = (maxLPSCenterPosition - maxLPSLength)/2;
    end = start + maxLPSLength - 1;
    stringstream ss;
    for (i = start; i <= end; ++i) ss << str[i];
    return ss.str();
}
```

### Subset Sum Problem

```text
Given array = [1 2 4 5 9]
     0     1     2     3     4     5    6    7    8    9    10    11
1    T     T     F     F     F     F
2    T     T     T     T     F
4
5
9
```

### Knapsack

```text
Total Weight: 7
Items (weight-val): 1-1, 3-4, 4-5, 5-7

            (total weight capacity of knapsack)
Val (Weight)    0   1   2   3   4   5   6   7
    1 (1)       0   1   1   1   1   1   1   1   [we only have 1 quantity of
    4 (3)       0   1   1   4   5   5   5   5    each item, other varient could
    5 (4)       0   1   1   4   5  *6*  6   9    could be having as many item]
    7 (5)       0   1   1   4   5   7   8   9
```

### [Pizza with 3n Slices](https://leetcode.com/problems/pizza-with-3n-slices/)

```cpp
vector<vector<int>> dp;
int totalCnt;
int solve(vector<int> &slices, int cur = 0, int cnt = 0)
{
    if (cur >= slices.size() || cnt >= totalCnt) return 0;
    if (dp[cur][cnt] != -1) return dp[cur][cnt];

    int res = max(slices[cur] + solve(slices, cur+2, cnt+1), solve(slices, cur+1, cnt));
    return dp[cur][cnt] = res;
}
int maxSizeSlices(vector<int>& slices)
{
    dp = vector<vector<int>>(slices.size(), vector<int>(slices.size()/3, -1));
    totalCnt = slices.size()/3;
    vector<int> x = slices, y = slices;
    x.erase(x.begin()); y.erase(y.end()-1);
    int res = solve(x);
    dp = vector<vector<int>>(slices.size(), vector<int>(slices.size()/3, -1));
    res = max(res, solve(y));
    return res;
}
```

### Maximum Submatrix with sides x

```cpp
/* mat[6][6] =  X  O  X  X  X  X
                X  O  X  X  O  X
                X  X  X  O  O  X
                O  X  X  X  X  X
                X  X  X  O  X  O
                O  O  X  O  O  O
DP[6][6] =   [1,1]  [0,0]  [1,1]  [2,1]  [3,1]  [4,1]
             [1,2]  [0,0]  [1,2]  [2,2]  [0,0]  [1,2]
             [1,3]  [2,1]  [3,3]  [0,0]  [0,0]  [1,3]
             [0,0]  [1,2]  [2,4]  [3,1]  [4,1]  [5,4]
             [1,1]  [2,3]  [3,5]  [0,0]  [1,2]  [0,0]
             [0,0]  [0,0]  [1,6]  [0,0]  [0,0]  [0,0]

After that: (zero-indexed) */
int res = 0;
for (int i = n-1; i >= 1; --i)
{
    for (int j = n-1; j >= 1; --j)
    {
        int cur = getMin(DP[i][j].F, DP[i][j].S);
        while (cur > res)
        {
            if (DP[i][j-cur+1].S >= cur && DP[i-cur+1][j].F >= cur) res = cur;
            cur--;
        }
    }
}
return res;
```

### Kaden's Algorithm on Product

```cpp
int maxProduct(vector<int>& nums)
{
    int ans = nums[0], maxToHere = nums[0], minToHere = nums[0];
    for (int i = 1; i < nums.size(); ++i)
    {
        int prevMaxToHere = maxToHere;
        int x = nums[i];
        maxToHere = max({minToHere * x, prevMaxToHere * x, x});
        minToHere = min({minToHere * x, prevMaxToHere * x, x});
        ans = max(ans, maxToHere);
    }
    return ans;
}
```

### Kaden to find Maximum Sum Sub-matrix

```cpp
/*
 2    1   -3  -4    5               2   3   0  -4    1
 0    6    3   4    1               0   6   9   13   14
 2   -2   -1   4   -5               2   0  -1   3    -2
-3    3    1   0    3              -3   0   1   1    4
                              [Horizontal prefix sum matrix]
Idea is to reduce N^4 brute force to N^3 using kaden (so we are converting
problem to Kaden) Iterate over all possible L, R and then using prefixsm mat
find a single column mat on which apply kaden to find maximum.
```

### [Egg Dropping Puzzle](https://leetcode.com/problems/super-egg-drop/)

There are n floors and k eggs if we drop an egg from a floor and it breaks we cannot reuse it otherwise we can, What is the minimum number of moves required to find at which floor egg breaks.

![](.gitbook/assets/image%20%2843%29.png)

```cpp
/* Naive DP: 1 + min_for_x_1_to_N(max(DP[e-1, x-1], DP[e, n-x]))
It's straightforward, if we throw egg it breaks means we have e-1 eggs
and n-1 floors otherwise n-1 floors and e eggs. This leads us to O(KN^2) solution.

Now DP[k, n] function is monotonic increasing on N because of that
T1: DP[k-1, x-1] is also increasing but T2: DP[k, n-x] is decreasing on x.
[Refer image]
T1 < T2 (x is too small)    T1 > T2 (x is too large)
So apply binary search accordingly. */
vector<vector<int>> dp;
int solve(int k, int n)
{
    if (n == 0) return 0;
    if (k == 1) return n;
    if (dp[k][n] != -1) return dp[k][n];

    // NAIVE WAY TO TRANSITION
    int res = INT_MAX;
    for (int i = 1; i <= n; ++i)
        res = min(res, 1 + max(solve(k-1, i-1), solve(k, n-i)));
    // NAIVE WAY ENDS

    // BINARY SEARCH OPTIMIZATION IN TRANSITION
    int l = 1, r = n;
    while (l < r)
    {
        int mid = l + (r-l)/2;
        if (solve(k-1, mid-1) < solve(k, n-mid)) l = mid+1;
        else r = mid;
    }
    int res = 1 + max(solve(k-1, l-1), solve(k, n-l));
    // BINARY SEARCH WAY ENDS HERE

    return dp[k][n] = res;
}
int superEggDrop(int k, int n)
{
    dp = vector<vector<int>>(k+1, vector<int>(n+1, -1));
    return solve(k, n);
}
```

### **Text Justification**

```cpp
//Greedy
vector<string> fullJustify(vector<string>& words, int maxWidth)
{
    vector<string> res, cur;
    int len = 0;
    string str;
    for (int i = 0; i < words.size(); ++i)
    {
        if (len+cur.size()+words[i].size() <= maxWidth)
        {
            cur.push_back(words[i]);
            len += words[i].size();
        }
        else
        {
            if (cur.size() == 1)
            {
                str = cur[0];
                str.append(maxWidth - str.size(), ' ');
            }
            else
            {
                int div = (maxWidth - len) / (cur.size() - 1);
                int mod = (maxWidth - len) % (cur.size() - 1);
                str = cur[0];
                for (int j = 1; j < cur.size(); ++j)
                {
                    if (j <= mod) str.append(div+1, ' ');
                    else str.append(div, ' ');
                    str += cur[j];
                }
            }
            res.push_back(str);
            cur.clear();
            cur.push_back(words[i]);
            len = words[i].size();
        }
    }

    // Last line, add space in the right side
    str = cur[0];
    for (int j = 1; j < cur.size(); ++j) str += ' ' + cur[j];
    str.append(maxWidth - str.size(), ' ');
    res.push_back(str);
    return res;
}

/* Given: Tushar Roy likes to code    Width: 10
              spaces
Tushar Roy  ->   0
likes to    ->   2
code        ->   6

              spaces
Tushar      ->   4
Roy likes   ->   1
to code     ->   3

we check badness of arrangement by space left squared sum
(we could have even taken cube it's just to make it sensitive to minor errors)
so 40 & 26. Greedy approach of fitting as many words will fail.

This dp represents badness of picking i-j elements to display on one line
of given width
    0   1   2   3   4
0   16  0   INF INF INF
1       49  1   INF INF
2           25  4   INF
3               64  9
4                   36

Now we will create another dp table that will keep track of minimum badness for
each new word we put
[16, 0, 17, 4, 26]
last index has the answer of total badness */
```

### Interleaving Strings

```cpp
/* Given three strings A, B and C. Write a function that
checks whether C is an interleaving of A and B. C is
said to be interleaving A and B, if it contains all
characters of A and B and order of all characters in
individual strings is preserved. */

// Simple Solution
bool isInterleave(string a, string b, string c)
{
    int i = 0, j = 0, k = 0;
    while (k < c.size())
    {
        if (a[i] == c[k]) ++i;
        else if (b[j] == c[k]) ++j;
        else return false;
        ++k;
    }
    if (i < a.size()-1 || j < b.size()-1) return false;
    return true;
}
/* Simple solution doesn’t work if strings A and B have some common characters.
This recursive solution works but works in 2^n complexity. Memoizing requres i, j,
cur all three there are no overlapping subproblems directly if we convert it */
class Solution {
public:
    vector<vector<int>> dp;
    bool check(string &s1, string &s2, string &s3, int i = 0, int j = 0)
    {
        int cur = i + j;
        if (i == s1.size() && j == s2.size()) return true;
        if (dp[i][j] != -1) return dp[i][j];
        
        bool res = false;
        if (s3[cur] == s1[i]) res |= check(s1, s2, s3, i+1, j);
        if (s3[cur] == s2[j]) res |= check(s1, s2, s3, i, j+1);
        return dp[i][j] = res;
    }
    bool isInterleave(string s1, string s2, string s3)
    {
        dp = vector<vector<int>>(s1.size()+1, vector<int>(s2.size()+1, -1));
        if (s1.size() + s2.size() != s3.size()) return false;
        return check(s1, s2, s3);
    }
};
​
// Bottom up DP Approach
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3)
    {
        int len1 = s1.size(), len2 = s2.size(), len3 = s3.size();
        if (len1 + len2 != len3) return false;
​
        // dp[i][j] means s3[0 to i+j-1] is interleaving for s1[0 to i-1] and s2[0 to j-1]
        vector< vector<bool> > dp(len1 + 1, vector<bool>(len2 + 1, false));
        dp[0][0] = true;
        for (int i = 1; i <= len1; ++i)
            dp[i][0] = (s1[i-1] == s3[i-1]) ? dp[i-1][0] : false;
        for (int i = 1; i <= len2; ++i)
            dp[0][i] = (s2[i-1] == s3[i-1]) ? dp[0][i-1] : false;
        for (int i = 1; i <= len1; ++i)
        {
            for (int j = 1; j <= len2; ++j)
            {
                // Current character of s3 matches with current character of
                // s1 but not with s2
                if (s1[i-1] == s3[i+j-1] && s2[j-1] != s3[i+j-1])
                    dp[i][j] = dp[i-1][j];
                else if (s1[i-1] != s3[i+j-1] && s2[j-1] == s3[i+j-1])
                    dp[i][j] = dp[i][j-1];
                else if (s1[i-1] == s3[i+j-1] && s2[j-1] == s3[i+j-1])
                    dp[i][j] = (dp[i][j-1] || dp[i-1][j]);
            }
        }
        return dp[len1][len2];
    }
};
```

### [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

```cpp
int longestValidParentheses(string s)
{
    int n = s.size();
    vector<int> dp(n, 0);
    int ans = 0;
    for (int i = 1; i < n; ++i)
    {
        if (s[i] == ')')
        {
            // ()
            if (s[i-1] == '(')
                dp[i] = (i >= 2) ? dp[i-2] + 2 : 2;
            // ()(())   =   020026
            else if (i-dp[i-1]-1 >= 0 && s[i-dp[i-1]-1] == '(')
                dp[i] = (i-dp[i-1] >= 2) ?
                    dp[i-1] + dp[i-dp[i-1]-2] + 2 : dp[i-1] + 2;
        }
        ans = max(ans, dp[i]);
    }
    return ans;
}
```

### [Minimum Swaps To Make Sequences Increasing](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/)
```c++
vector<vector<int>> dp;
int solve(vector<int> &A, vector<int> &B, int cur = 0, bool rev = false)
{
    if (cur == A.size()) return 0;
    if (dp[cur][rev] != -1) return dp[cur][rev];
    
    int p1 = (cur == 0) ? INT_MIN : A[cur-1], p2 = A[cur];
    int q1 = (cur == 0) ? INT_MIN : B[cur-1], q2 = B[cur];
    if (rev) swap(p1, q1);
    
    int res = INT_MAX;
    if (p1 < p2 && q1 < q2) res = min(res, solve(A, B, cur+1, false));
    if (p1 < q2 && q1 < p2) res = min(res, 1+solve(A, B, cur+1, true));

    return dp[cur][rev] = res;
}
int minSwap(vector<int>& A, vector<int>& B)
{
    dp = vector<vector<int>>(A.size(), vector<int>(2, -1));
    return solve(A, B);
}
```

### Word Break

* [https://leetcode.com/problems/word-break/](https://leetcode.com/problems/word-break/)
* [https://leetcode.com/problems/word-break-ii/](https://leetcode.com/problems/word-break-ii/)

```cpp
bool wordBreak(string s, vector<string>& wordDict)
{
    unordered_set<string> rec;
    for (string s : wordDict) rec.insert(s);
    string cur = "";
    for (int i = 0; i < s.size(); ++i)
    {
        cur += s[i];
        if (i == s.size()-1)
            return (rec.find(cur) != rec.end());
        if (rec.find(cur) != rec.end())
            cur = "";
    }
    return false;
}
/* Above greedy solution won't work for this testcase 
"aaaaaaa" ["aaaa","aaa"], DP solution is needed */
vector<int> dp;
bool solve(string_view &s, unordered_set<string_view> &rec, int cur = 0)
{
    if (cur == s.size()) return true;
    if (dp[cur] != -1) return dp[cur];

    for (int i = cur; i < s.size(); ++i)
    {
        if (rec.find(s.substr(cur, i-cur+1)) != rec.end())
            if (solve(s, rec, i+1)) return dp[cur] = true;
    }
    return dp[cur] = false;
}
bool wordBreak(string s, vector<string>& wordDict)
{
    unordered_set<string_view> rec(wordDict.begin(), wordDict.end());
    dp = vector<int>(s.size(), -1);
    string_view str(s);
    return solve(str, rec);
}

// The HARD variation also wants you to show every possible finding
class Solution {
public:
    vector<int> dp;
    vector<vector<int>> vals;
    bool solve(string &s, unordered_map<string, int> &rec, int cur = 0)
    {
        if (cur == s.size()) return true;
        if (dp[cur] != -1) return dp[cur];

        bool res = false;
        for (int i = cur; i < s.size(); ++i)
        {
            string x = s.substr(cur, i-cur+1);
            auto ind = rec.find(x);
            if (ind != rec.end() && solve(s, rec, i+1))
            {
                res = true;
                vals[cur].push_back(ind->second);
            }
        }
        return dp[cur] = res;
    }
    vector<string> curAns;
    void dfs(vector<string> &wordDict, vector<string> &ans, int cur = 0)
    {
        if (cur == vals.size())
        {
            string finalAns = "";
            for (auto &x : curAns) finalAns += x + ' ';
            ans.push_back(finalAns.substr(0, finalAns.size()-1));
            return;
        }
        for (auto &x : vals[cur])
        {
            curAns.push_back(wordDict[x]);
            dfs(wordDict, ans, cur + wordDict[x].size());
            curAns.pop_back();
        }
    }
    vector<string> wordBreak(string s, vector<string>& wordDict)
    {
        unordered_map<string, int> rec;
        for (int i = 0; i < wordDict.size(); ++i) rec[wordDict[i]] = i;
        dp.assign(s.size(), -1);
        vals.resize(s.size());
        solve(s, rec);
        
        vector<string> ans;
        curAns.clear();
        dfs(wordDict, ans);
        return ans;
    }
};
```

### [Guess Number Higher or Lower II](https://leetcode.com/problems/guess-number-higher-or-lower-ii/)
```c++
vector<vector<int>> dp;
int solve(int l, int r)
{
    if (l >= r) return 0;
    if (dp[l][r] != -1) return dp[l][r];

    int res = INT_MAX;
    for (int i = l; i <= r; ++i)
        res = min(res, i+max(solve(l, i-1), solve(i+1, r)));
    return dp[l][r] = res;
}
int getMoneyAmount(int n)
{
    dp = vector<vector<int>>(n+1, vector<int>(n+1, -1));
    return solve(1, n);
}
```

### Number of Subsequences of form a^i b^j c^k

If given string is sorted in a b c order then ans would have been simply: \(2^cnta - 1\) \* \(2^cntb - 1\) \* \(2^cntc - 1\)  
2^cnt - 1 because we are doing nCr for all r except r = 1 \(n = cnt here\) this is equal to 2^cnt - 1

```cpp
/* If current char is 'a' it can mean - ch begins a new seq., it's part of an
ongoing a's, not any of the two. */
string str; cin >> str;
int aCnt = 0, bCnt = 0, cCnt = 0;
for (auto &ch : str)
{
    if (ch == 'a') aCnt = (1 + 2*aCnt);
    else if (ch == 'b') bCnt = (aCnt + 2*bCnt);
    else if (ch == 'c') cCnt = (bCnt + 2*cCnt);
}
cout << cCnt << '\n';
```

### Best time to buy and sell stock

```cpp
/* I: Consider it as a line graph, we want to buy at valley and sell
at peak */
int maxProfit(vector<int>& prices)
{
    int mn = INT_MAX;
    int ans = 0;
    for (auto &x : prices)        
    {
        mn = min(mn, x);
        ans = max(ans, x-mn);
    }
    return ans;
}

/* II: As many transaction as possible, Continuing upon previous idea we will
sell for every consecutive valley and peak */
int maxProfit(vector<int>& prices)
{
    if (prices.size() <= 1) return 0;
    int n = prices.size();
    int ans = 0;
    for (int i = 0; i < n-1; ++i)
    {
        while (i < n-1 && prices[i] > prices[i+1]) i++;
        int valley = prices[i];
        while (i < n-1 && prices[i] < prices[i+1]) i++;
        int peak = prices[i];
        ans += peak-valley;
    }
    return ans;
}

/* III: At most 2 transactions, it's extension of variation 1 though here
constraint optimizing is done keeping 4 parameters. */
int maxProfit(vector<int>& prices)
{
    int buy1 = INT_MIN, buy2 = INT_MIN, sell1 = 0, sell2 = 0;
    for (auto &x : prices)
    {
        buy1 = max(buy1, -x);    // Keeping profit in mind that's why -x
        sell1 = max(sell1, buy1 + x);
        buy2 = max(buy2, sell1 - x);
        sell2 = max(sell2, buy2 + x);
    }
    return sell2;
}

/* IV: At most k transactions, extending idea of IIIrd */
int maxProfit(int k, vector<int>& prices)
{
    if (k >= prices.size()/2)
    {
        int ans = 0;
        for (int i = 1; i < prices.size(); ++i)
            if (prices[i] > prices[i-1]) ans += prices[i] - prices[i-1];
        return ans;
    }
    vector<int> buy(k+1, INT_MIN), sell(k+1, 0);
    for (int &x : prices)
    {
        for (int i = 1; i <= k; ++i)
        {
            buy[i] = max(buy[i], sell[i-1] - x);
            sell[i] = max(sell[i], buy[i] + x);
        }
    }
    return sell[k];
}

/* V: With cooldown, top down version */
int solve(vector<int> &prices, vector<int> &memo, int cur = 0)
{
    if (cur >= prices.size()) return 0;
    if (memo[cur] != -1) return memo[cur];
    int mn = INT_MAX;
    int res = 0;
    for (int i = cur; i < prices.size(); ++i)
    {
        mn = min(mn, prices[i]);
        res = max(res, (prices[i] - mn) + solve(prices, memo, i+2));
    }
    return memo[cur] = res;
}
int maxProfit(vector<int>& prices)
{
    if (prices.size() <= 1) return 0;
    vector<int> memo(prices.size(), -1);
    return solve(prices, memo);
}

/* Bottom up version, Consider a 2D DP with n columns and 3 rows:
0 (No transaction on hold) 1 (Currently bought 1) 2 (Sold off) */
int maxProfit(vector<int>& prices)
{
    int n = prices.size();
    if (n <= 1) return 0;
    int dp[3] = {0, -prices[0], INT_MIN};
    for (int i = 1; i < n; ++i)
    {
        int x = max(dp[0], dp[2]);
        int y = max(dp[1], dp[0] - prices[i]);
        int z = max(dp[2], dp[1] + prices[i]);
        dp[0] = x, dp[1] = y, dp[2] = z;
    }
    return max(dp[0], dp[2]);
}

/* VI: With transaction fees */
int maxProfit(vector<int>& prices, int fee)
{
    int n = prices.size();
    if (n <= 1) return 0;
    int dp[3] = {0, -prices[0], -(1<<30)};
    for (int i = 0; i < n; ++i)
    {
        dp[0] = max(dp[0], dp[2] - fee);
        dp[1] = max(dp[1], dp[0] - prices[i]);
        dp[2] = max(dp[2], dp[1] + prices[i]);
    }
    return max(dp[0], dp[2]-fee);
}
```

### House Robber
- https://leetcode.com/problems/house-robber/
- https://leetcode.com/problems/house-robber-ii/
- https://leetcode.com/problems/house-robber-iii/
```c++
// Variant - I (No adjacent house to rob allowed)
class Solution {
public:
    vector<int> dp;
    int solve(vector<int> &nums, int cur = 0)
    {
        if (cur >= nums.size()) return 0;
        if (dp[cur] != -1) return dp[cur];
        return dp[cur] = max(solve(nums, cur+1), nums[cur] + solve(nums, cur+2));
    }
    int rob(vector<int>& nums)
    {
        dp = vector<int> (nums.size(), -1);
        return solve(nums);
    }
};

// Variant - II (Array is cyclic)
class Solution {
public:
    vector<int> dp;
    int solve(vector<int> &nums, int l, const int r)
    {
        if (l >= r) return 0;
        if (dp[l] != -1) return dp[l];
        return dp[l] = max(solve(nums, l+1, r), nums[l] + solve(nums, l+2, r));
    }
    int rob(vector<int>& nums)
    {
        if (nums.size() == 1) return nums[0];
        if (nums.size() == 2) return max(nums[0], nums[1]);
        dp = vector<int> (nums.size(), -1);
        int x = solve(nums, 0, nums.size()-1);
        dp = vector<int> (nums.size(), -1);
        int y = solve(nums, 1, nums.size());
        return max(x, y);
    }
};

// Variant - III (Now it's a binary tree)
class Solution {
public:
    unordered_map<TreeNode*, int> dp;
    int solve(TreeNode* root)
    {
        if (!root) return 0;
        if (dp.find(root) != dp.end()) return dp[root];
        
        int res = root->val;
        if (root->left) res += solve(root->left->left) + solve(root->left->right);
        if (root->right) res += solve(root->right->left) + solve(root->right->right);
        
        res = max(res, solve(root->left) + solve(root->right));
        return dp[root] = res;
    }
    int rob(TreeNode* root)
    {
        return solve(root);
    }
};
```

### Weighted Job Scheduling Problem

Given jobs along with their weights we need to choose such that we get max weight out of it.

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, tuple<int, int, int>> arr(n);
    for (auto &x : arr) { int s, e, w; cin >> s >> e >> w; x = {e, s, w}; }
    sort(all(arr));

    vec<1, int> dp(n);
    dp[0] = get<2>(arr[0]);
    for (int i = 1; i < n; ++i)
    {
        int e = get<0>(arr[i]), s = get<1>(arr[i]), w = get<2>(arr[i]);
        int optimal = lower_bound(all(arr), make_tuple(s, 0, 0)) - arr.begin() - 1;
        if (optimal >= 0) dp[i] = max({dp[i-1], dp[optimal] + w, w});
        else dp[i] = max(dp[i-1], w);
    }
    cout << dp[n-1] << '\n';
    return 0;
}
```

### Wildcard & Regex Matching

* [https://leetcode.com/problems/wildcard-matching](https://leetcode.com/problems/wildcard-matching)
* [https://leetcode.com/problems/regular-expression-matching](https://leetcode.com/problems/regular-expression-matching)

```cpp
/* Wildcard Matching
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence). */
bool isMatch(string s, string p)
{
    vector<vector<bool>> dp(s.size()+1, vector<bool>(p.size()+1, false));
    dp[0][0] = true;
    for (int i = 1; i <= s.size(); ++i) dp[i][0] = false;
    for (int i = 1; i <= p.size(); ++i) dp[0][i] = (p[i-1] == '*') ? dp[0][i-1] : false;
    for (int i = 1; i <= s.size(); ++i)
    {
        for (int j = 1; j <= p.size(); ++j)
        {
            if (p[j-1] == '?') dp[i][j] = dp[i-1][j-1];
            else if (p[j-1] == '*') dp[i][j] = dp[i-1][j] || dp[i][j-1] || dp[i-1][j-1];
            else dp[i][j] = (s[i-1] == p[j-1]) ? dp[i-1][j-1] : false;
        }
    }
    return dp[s.size()][p.size()];
}

/* Regex Matching
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
aab -> c*a*b -> true c can be repeated 0 times, a 1 then b

      ''    c    *    a    *    b
''    T     F    T    F    T    F
a     F     F    F    T    T    F
a     F     F    F    F    T    F
b     F     F    F    F    F    T */
bool isMatch(string s, string p)
{
    int m = s.size(), n = p.size();
    vector< vector<bool> > dp(m+1, vector<bool>(m+1, false));
    dp[0][0] = true;
    for (int i = 1; i <= m; ++i) dp[i][0] = false;
    for (int j = 1; j <= n; ++j) dp[0][j] = (j > 1 && p[j-1] == '*' && dp[0][j-2]);
    for (int i = 1; i <= m; ++i)
    {
        for (int j = 1; j <= n; ++j)
        {
            if (p[j-1] != '*')
                dp[i][j] = (dp[i-1][j-1] && (s[i-1] == p[j-1] || p[j-1] == '.'));
            else
                dp[i][j] = (dp[i][j-2] || ((s[i-1] == p[j-2] || p[j-2] == '.') && dp[i-1][j]));
        }
    }
    return dp[m][n];
}

```

### [Different Ways to Add Paranthesis](https://leetcode.com/problems/different-ways-to-add-parentheses/)
```c++
unordered_map<string, vector<int>> dp;
vector<int> solve(string input)
{
    if (dp.find(input) != dp.end()) return dp[input];

    vector<int> res;
    for (int i = 0; i < input.size(); ++i)
    {
        char ch = input[i];
        if (ch == '+' || ch == '-' || ch == '*')
        {
            for (const int a : diffWaysToCompute(input.substr(0, i)))
            {
                for (const int b : diffWaysToCompute(input.substr(i+1)))
                {
                    if (ch == '+') res.push_back(a + b);
                    else if (ch == '-') res.push_back(a - b);
                    else if (ch == '*') res.push_back(a * b);
                }
            }       
        }
    }
    // if input string contains only number
    if (res.empty()) res.push_back(stoi(input));

    return dp[input] = res;
}
vector<int> diffWaysToCompute(string input)
{
    dp.clear();
    return solve(input);
}
```

### Travelling Salesman Problem

Salesman travels over a set of cities, he has to return to source. Minimize total distance travelled by him. Such cycle is also called hamiltonian cycle. We want minimum weight hamiltonian cycle. we will use bitmask to make it efficient. 000001 means out of all 6 cities 1st is visited.

![](.gitbook/assets/image%20%28155%29.png)

```cpp
#include <bits/stdc++.h>
using namespace std;

int n = 4;
int dist[4][4] =
{
    { 0,  20, 42, 25},
    { 20,  0, 30, 34},
    { 42, 30,  0, 10},
    { 25, 34, 10,  0}
};
int VISITED_ALL = (1<<n) -1;
int dp[16][4];

int tsp (int mask, int pos)
{
    if (mask == VISITED_ALL) return dist[pos][0];
    if (dp[mask][pos] != -1) return dp[mask][pos];

    int ans = INT_MAX;
    for (int city = 0; city < n; ++city)
    {
        if ((mask&(1<<city)) == 0)
            ans = min(dist[pos][city] + tsp(mask|(1<<city), city), ans);
    }

    dp[mask][pos] = ans;
    return ans;
}

int main()
{
    for (int i = 0; i < (1<<n); ++i)
    {
        for (int j = 0; j < n; ++j)
            dp[i][j] = -1;
    }
    cout << tsp(1, 0);
    return 0;
}
```

### [Binary Tree Camera](https://leetcode.com/problems/binary-tree-cameras/)

```cpp
/* Binary Tree Cameras
Given a binary tree, we install cameras on the nodes of the tree.
Each camera at a node can monitor its parent, itself, and its immediate children.
Calculate the minimum number of cameras needed to monitor all nodes of the tree. */
class Solution {
public:
    // root->val is used to memoize
    int minCameraCover(TreeNode* root,
        bool hasCam = false, bool isMonitored = false)
    {
        if (!root) return 0;
        if (hasCam)
            return 1 + minCameraCover(root->left, false, true) + 
                minCameraCover(root->right, false, true);

        if (isMonitored)
        {
            int noCam = minCameraCover(root->left, false, false) +
                minCameraCover(root->right, false, false);
            int rootCam = 1 + minCameraCover(root->left, false, true) +
                minCameraCover(root->right, false, true);
            return min(noCam, rootCam);
        }

        if (root->val != 0) return root->val;

        int rootCam = 1 + minCameraCover(root->left, false, true) +
            minCameraCover(root->right, false, true);
        int leftCam = (root->left)
            ? minCameraCover(root->left, true, false) +
                minCameraCover(root->right, false, false): INT_MAX;
        int rightCam = (root->right)
            ? minCameraCover(root->left, false, false) +
                minCameraCover(root->right, true, false) : INT_MAX;
        return root->val = min({rootCam, leftCam, rightCam});
    }
};
```

### [Cherry Pickup](https://leetcode.com/problems/cherry-pickup)

```cpp
/* Greedy method of doing first traverse from top left
to bottom right then bottom right to top left fails! */

/* Top down DP approach
We basically have to deal with both case together, we can
reverse the second path because direction won't matter so
we want to find two paths from top left to bottom right
giving maximum count.

classical one path problem stores states like: dp[r][c] is
maximum cherries that can be picked at r row and c cloumn.
Now we have r1, c1, r2, c2. At any point of traversal
r1 + c1 = r2 + c2 using this we can avoid one term
so, r2 = r1 + c1 - c2
now we can form simmilar dp[r1][c1][c2] */
int n, dp[55][55][55];
int compute(vector<vector<int>> &grid, int r1, int c1, int c2)
{
    int r2 = r1+c1-c2;
    if (r1 == n || r2 == n || c1 == n || c2 == n || grid[r1][c1] == -1 || grid[r2][c2] == -1) return INT_MIN;
    if (r1 == n-1 && c1 == n-1) return grid[r1][c1];
    if (dp[r1][c1][c2] != INT_MIN) return dp[r1][c1][c2];
    
    int ans = grid[r1][c1];
    if (c1 != c2) ans += grid[r2][c2];
    ans += max({compute(grid, r1, c1+1, c2+1), compute(grid, r1+1, c1, c2+1),
        compute(grid, r1, c1+1, c2), compute(grid, r1+1, c1, c2)});
    dp[r1][c1][c2] = ans;
    return ans;
}
int cherryPickup(vector<vector<int>>& grid)
{
    n = grid.size();
    for (int i = 0; i < 55; ++i) for (int j = 0; j < 55; ++j) for (int k = 0; k < 55; ++k) dp[i][j][k] = INT_MIN;
    return max(0, compute(grid, 0, 0, 0));
}
```

### [Number of Music Playlist](https://leetcode.com/problems/number-of-music-playlists/)

First, what is important in deciding a state? Naive way is to store for every number \(N\) then previous appearance value but this means N+1 states which is insane. To reduce states we have to apply some mathematical relation. After observation, if we have x length of playlist in which y are unique and rest are repeating. So if we have total z ways to have such state then in transitioning:

* Either we pick a song which is not unique. Multiply by unique-K because we can pick any such song since the gap of K is satisfied.
* We are picking a unique song, Multiply it by N-unique because we can pick any such non picked unique song.

```cpp
class Solution {
public:
    #define ll long long
    const int MOD = 1e9+7;
    vector<vector<int>> dp;
    ll solve(int &N, int &L, int &K, int cur = 0, int unique = 0)
    {
        if (cur == L) return unique == N;
        if (dp[cur][unique] != -1) return dp[cur][unique];
        
        ll res = (solve(N, L, K, cur+1, unique) * max(0, unique-K)) % MOD;
        (res += (solve(N, L, K, cur+1, unique+1) * (N-unique)) % MOD) %= MOD;
        return dp[cur][unique] = res;
    }
    int numMusicPlaylists(int N, int L, int K)
    {
        dp.assign(L, vector<int>(L, -1));
        return solve(N, L, K);
    }
};
```

### [The Maze](https://www.lintcode.com/problem/the-maze/description)

```cpp
class Solution {
public:
    vector<set<int>> inRow, inCol;
    vector<set<int, greater<int>>> inRowG, inColG;
    int n, m;
    pair<int, int> getPos(int r, int c, int dir)
    {
        if (dir == 0) // north
        {
            auto it = inColG[c].lower_bound(r);
            return (it == inColG[c].end()) ? make_pair(0, c) : make_pair(*it+1, c);
        }
        else if (dir == 1) // east
        {
            auto it = inRow[r].lower_bound(c);
            return (it == inRow[r].end()) ? make_pair(r, m-1) : make_pair(r, *it-1);
        }
        else if (dir == 2) // south
        {
            auto it = inCol[c].lower_bound(r);
            return (it == inCol[c].end()) ? make_pair(n-1, c) : make_pair(*it-1, c);
        }
        else // west
        {
            auto it = inRowG[r].lower_bound(c);
            return (it == inRowG[r].end()) ? make_pair(r, 0) : make_pair(r, *it+1);
        }
    }
    vector<vector<int>> dp;
    bool canReach(vector<int> &end, pair<int, int> cur)
    {
        if (cur.first == end[0] && cur.second == end[1]) return true;
        if (dp[cur.first][cur.second] != -1) return dp[cur.first][cur.second];
        
        dp[cur.first][cur.second] = false;
        for (int dir : {0, 1, 2, 3})
        {
            if (canReach(end, getPos(cur.first, cur.second, dir))) return dp[cur.first][cur.second] = true;
        }
        return false;
    }
    bool hasPath(vector<vector<int>> &maze, vector<int> &start, vector<int> &destination)
    {
        n = maze.size(), m = maze[0].size();
        dp.resize(n, vector<int>(m, -1)); inRow.resize(n); inCol.resize(m); inRowG.resize(n); inColG.resize(m);
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                if (maze[i][j] == 1) inRow[i].insert(j), inCol[j].insert(i), inRowG[i].insert(j), inColG[j].insert(i);
        return canReach(destination, {start[0], start[1]});
    }
};
```

### [Minimum Cost From Leaf Values](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/)

```cpp
/* Input: arr = [6,2,4]
Output: 32
Explanation:
There are two possible trees.  The first has
non-leaf node sum 36, and the second has non-leaf
node sum 32.

    24            24
   /  \          /  \
  12   4        6    8
 /  \               / \
6    2             2   4

Merge Binary trees pattern

Greedy approach using priority queue won't work.

This is kinda simillar to a classical DP optimization problem
but not exactly, since we can have any pair combined and not
just consecutive ones.

We can basically start with root and split optimally */
vector<vector<int>> dp;
const int INF = (1<<30);
int solve(vector<vector<int>> &maxBetween, int l, int r)
{
    if (l == r) return 0;   // leaf node
    if (dp[l][r] != -1) return dp[l][r];

    int res = INF;
    for (int i = l; i < r; ++i)
        res = min(res, solve(maxBetween, l, i) + solve(maxBetween, i+1, r) +
                    maxBetween[l][i]*maxBetween[i+1][r]);
    return dp[l][r] = res;
}
int mctFromLeafValues(vector<int>& arr)
{
    vector<vector<int>> maxBetween(arr.size(), vector<int>(arr.size()));
    for (int i = 0; i < arr.size(); ++i)
    {
        maxBetween[i][i] = arr[i];
        for (int j = i+1; j < arr.size(); ++j)
            maxBetween[i][j] = max(maxBetween[i][j-1], arr[j]);
    }
    dp = vector<vector<int>>(arr.size(), vector<int>(arr.size(), -1));
    return solve(maxBetween, 0, arr.size()-1);
}
```

### Strobogrammatic Number

* [https://www.lintcode.com/problem/strobogrammatic-number/description](https://www.lintcode.com/problem/strobogrammatic-number/description)
* [https://www.lintcode.com/problem/strobogrammatic-number-ii/description](https://www.lintcode.com/problem/strobogrammatic-number-ii/description)
* [https://leetcode.com/problems/confusing-number/](https://leetcode.com/problems/confusing-number/)
* [https://leetcode.com/problems/confusing-number-ii/](https://leetcode.com/problems/confusing-number-ii/)

```cpp
/* I - tell if given number is strobogrammatic i.e. looks the same when
rotated 180 degrees (looked at upside down): eg - 69, 88 */
bool isStrobogrammatic(string &num)
{
    unordered_map<char, char> mirrorChars;
    mirrorChars['0'] = '0', mirrorChars['1'] = '1', mirrorChars['6'] = '9';
    mirrorChars['8'] = '8', mirrorChars['9'] = '6';
    
    for (int l = 0, r = num.size()-1; l <= r; ++l, --r)
    {
        if (mirrorChars.find(num[l]) == mirrorChars.end() ||
            mirrorChars.find(num[r]) == mirrorChars.end() ||
            mirrorChars[num[l]] != num[r] ||
            mirrorChars[num[r]] != num[l])      // this line is redundant
            return false;
    }
    return true;
}
```

```cpp
// Find all strobogrammatic numbers that are of length = n.
vector<string> processString(const vector<string> &arr, int n, int total)
{
    vector<string> res;
    for (string s : arr)
    {
        /* no leading zero that's why n != total, if n = 4 then 1001 will
        also be there, for inner n = 2 total = 4 */
        if (n != total) res.push_back("0" + s + "0");
        res.push_back("1" + s + "1");
        res.push_back("8" + s + "8");
        res.push_back("6" + s + "9");
        res.push_back("9" + s + "6");
    }
    return res;
}
/* total is basically initial value of n (doesn't change) n changes
as per recurssion */
vector<string> findStrobogrammatic(int n, int total = -1)
{
    if (total == -1) total = n;
    if (n == 0) return {""};
    if (n == 1) return {"1", "8", "0"};
    return processString(findStrobogrammatic(n-2, total), n, total);
}

// Iterative
vector<string> findStrobogrammatic(int n)
{
    vector<string> res;
    if (n&1) res = {"0", "1", "8"};
    else res = {""};
    while (n > 1)
    {
        n -= 2;
        vector<string> tempRes;
        for (const string s : res)
        {
            if (n >= 2) tempRes.push_back('0' + s + '0');
            tempRes.push_back('1' + s + '1');
            tempRes.push_back('8' + s + '8');
            tempRes.push_back('6' + s + '9');
            tempRes.push_back('9' + s + '6');
        }
        swap(res, tempRes);
    }
    return res;
}
```

```cpp
/* count strobogrammatic between string low and string high.
One solution is iterate over [low.size(), high.size()] for each generate
all strobogrammatic number using above code then do comparison */
bool compare(string a, string b) { return a.size() == b.size() ? a >= b : a.size() > b.size(); }
int strobogrammaticInRange(string low, string high)
{
    int res = 0;
    for (int i = l.size(); i <= r.size(); ++i)
    {
        for (const string s : findStrobogrammatic(i))
            res += (compare(s, low) && compare(high, s));
    }
    return res;
}

// Other way is to use DFS
int rotate(int x)
{
    int res = 0;
    while (x)
    {
        int cur = x%10;
        if (cur == 6) cur = 9;
        else if (cur == 9) cur = 6;
        res = res*10 + cur;
        x /= 10;
    }
    return res;
}
void dfs(const int n, int &ans, int num = 0)
{
    if (num > n) return;
    if (rotate(num) == num) ++ans;        // Make != for confusing number II

    if (num) dfs(n, ans, num*10);
    dfs(n, ans, num*10 + 1);
    dfs(n, ans, num*10 + 6);
    dfs(n, ans, num*10 + 8);
    dfs(n, ans, num*10 + 9);
}
int countStrobogrammaticFrom1ToN(int n)
{
    int ans = 0;
    dfs(n, ans);
    return ans;
}
```

### Top Down to Bottom Up Conversion : [Student Attendance Record II](https://leetcode.com/problems/student-attendance-record-ii/)

```cpp
// Top down - TLE
const int MOD = 1e9+7;
vector<vector<vector<int>>> dp;
int solve(const int n, int curDate = 0, bool wasAbsent = false, int wasLate = 0)
{
    if (curDate == n) return 1;
    if (dp[curDate][wasAbsent][wasLate] != -1) return dp[curDate][wasAbsent][wasLate];

    int res = 0;
    res = (res + solve(n, curDate+1, wasAbsent, 0)) % MOD;              // present
    if (!wasAbsent) res = (res + solve(n, curDate+1, true, 0)) % MOD;   // absent
    if (wasLate < 2) res = (res + solve(n, curDate+1, wasAbsent, wasLate+1)) % MOD; // late
    return dp[curDate][wasAbsent][wasLate] = res;
}
int checkRecord(int n)
{
    dp = vector<vector<vector<int>>>(n, vector<vector<int>>(2, vector<int>(3, -1)));
    return solve(n);
}

// Bottom up
int checkRecord(int n)
{
    int dp[n+1][2][3];
    memset(dp, 0, sizeof(dp));
    dp[0][0][0] = 1;
    for (int day = 0; day < n; ++day)
    {
        for (bool wasAbsent : {false, true})
        {
            for (int lateDays : {0, 1, 2})
            {
                if (dp[day][wasAbsent][lateDays] == 0) continue;

                (dp[day+1][wasAbsent][0] += dp[day][wasAbsent][lateDays]) %= MOD;                  // present
                if (!wasAbsent)
                    (dp[day+1][true][0] += dp[day][wasAbsent][lateDays]) %= MOD;                   // absent
                if (lateDays < 2)
                    (dp[day+1][wasAbsent][lateDays+1] += dp[day][wasAbsent][lateDays]) %= MOD;     // late
            }
        }
    }
    
    int res = 0;
    for (bool wasAbsent : {false, true})
    {
        for (int lateDays : {0, 1, 2})
            (res += dp[n][wasAbsent][lateDays]) %= MOD;
    }
    return res;
}
```

## AtCoder DP Contest

* [A - Frog 1](https://atcoder.jp/contests/dp/tasks/dp_a): O\(1\) Space solution - [https://atcoder.jp/contests/dp/submissions/12201992](https://atcoder.jp/contests/dp/submissions/12201992)
* [B - Frog 2](https://atcoder.jp/contests/dp/tasks/dp_b): [https://atcoder.jp/contests/dp/submissions/12205560](https://atcoder.jp/contests/dp/submissions/12205560) \(Can be done in O\(K\) space\)
* [C - Vacation](https://atcoder.jp/contests/dp/tasks/dp_c): [https://atcoder.jp/contests/dp/submissions/12205672](https://atcoder.jp/contests/dp/submissions/12205672)
* [D - Knapsack 1](https://atcoder.jp/contests/dp/tasks/dp_d): [https://atcoder.jp/contests/dp/submissions/12205835](https://atcoder.jp/contests/dp/submissions/12205835) \(Can be done in O\(N\) space\)
* [H - Grid 1](https://atcoder.jp/contests/dp/tasks/dp_h): [https://atcoder.jp/contests/dp/submissions/12207877](https://atcoder.jp/contests/dp/submissions/12207877)
* [F - LCS](https://atcoder.jp/contests/dp/tasks/dp_f): [https://atcoder.jp/contests/dp/submissions/12208540](https://atcoder.jp/contests/dp/submissions/12208540)
* [E - Knapsack 2](https://atcoder.jp/contests/dp/tasks/dp_e): [https://atcoder.jp/contests/dp/submissions/12207798](https://atcoder.jp/contests/dp/submissions/12207798)
* [L - Deque](https://atcoder.jp/contests/dp/tasks/dp_l): [https://atcoder.jp/contests/dp/submissions/12218686](https://atcoder.jp/contests/dp/submissions/12218686)
* [I - Coins](https://atcoder.jp/contests/dp/tasks/dp_i): [https://atcoder.jp/contests/dp/submissions/12212430](https://atcoder.jp/contests/dp/submissions/12212430)
* [G - Longest Path](https://atcoder.jp/contests/dp/tasks/dp_g): [https://atcoder.jp/contests/dp/submissions/12213578](https://atcoder.jp/contests/dp/submissions/12213578) \(Apply topological sort then see ending at cur node what's the val DP\[v\] = max\(DP\[v\], DP\[u\] + 1\)
* [R - Walk](https://atcoder.jp/contests/dp/tasks/dp_r): Naive O\(N\*K\) solution which finds K walk using all neighbouring K-1 walks. However it won't work because K is huge here. Adj graph property of exponentiation works here. [https://atcoder.jp/contests/dp/submissions/12269213](https://atcoder.jp/contests/dp/submissions/12269213)
* [Y - Grid 2](https://atcoder.jp/contests/dp/tasks/dp_y): f is n+m-2 C m-1 \(Submission: [https://atcoder.jp/contests/dp/submissions/12289185](https://atcoder.jp/contests/dp/submissions/12289185)\)

```cpp
arr.push_back({h, w});
/* Defines our DP order those before i are definitely <= (first & second) */
sort(all(arr), [](auto x, auto y) {
    return x.F + x.S < y.F + y.S;
});

vec<1, int> DP(arr.size());
for (int i = 0; i < arr.size(); ++i)
{
    // All the ways to reach cur point without considering obstacles
    int res = f(arr[i].F, arr[i].S);
    for (int j = 0; j < i; ++j)
    {
        /* Check is imp because x+y will also hold true for y+x sorting was
        for defining order as all those <= will definitely be before leaving
        some exceptions. */
        if (arr[j].F <= arr[i].F && arr[j].S <= arr[i].S)
            res = (res - multiply(DP[j], f(arr[i].F-arr[j].F+1, arr[i].S-arr[j].S+1)) + MOD) % MOD;
    }
    /* Ways to reach that point times ways to reach cur from that point
    subtract this since this counts as obstacle */
    DP[i] = res;
}
cout << DP.back() << '\n';
```

* [K - Stones](https://atcoder.jp/contests/dp/tasks/dp_k) - This problem resembles subset sum problem in a way, applying that logic we can have 0...k stone piles for each we can choose n elements.

```text
n = 3, k = 2    [2 7 3]
DP:      0    1    2    3
    4    F    F    F    F        F means first player can't win
    2    F    F    T    T
    1    F    T    T    F

DP[i][j] = DP[i-1][j] | DP[i][j-arr[i]]
This relation has a problem that j-arr[i] term must be looked from final (last
row) so we need to perform operation column major form
        0    1    2    3
        F    T    T    F
Here DP[i] |= DP[i-arr[x]] where arr[x] is every item of arr
```

[https://atcoder.jp/contests/dp/submissions/12217943](https://atcoder.jp/contests/dp/submissions/12217943)

* [P - Independent Set](https://atcoder.jp/contests/dp/tasks/dp_p): Consider 1D DP of pairs first denoting combinations ending at that node \(ends at white\) and second \(ends at black\). [https://atcoder.jp/contests/dp/submissions/12231618](https://atcoder.jp/contests/dp/submissions/12231618)
* [M - Candies](https://atcoder.jp/contests/dp/tasks/dp_m): Create a 2D DP with 0..k columns and rows denoting array, now in DP\[i\]\[j\] we want to find min candies if we pick first i childrens and j candies what will be total possibilities, it will simply be DP\[i\]\[j\] += DP\[i-1\]\[j-x\] Where x is 0...arr\[i\] We can optimize this by prefix summing \(i-1\)th DP row, overall making it O\(N\*K\) [https://atcoder.jp/contests/dp/submissions/12221309](https://atcoder.jp/contests/dp/submissions/12221309)
* [Q - Flowers](https://atcoder.jp/contests/dp/tasks/dp_q): It's basically LIS with having maximal sum instead of count. Naive N^2 solution: [https://atcoder.jp/contests/dp/submissions/12255142](https://atcoder.jp/contests/dp/submissions/12255142) To optimize it to run in NlogN time use segment tree, Find max DP from height 0 to height\[i\]-1 [https://atcoder.jp/contests/dp/submissions/12268856](https://atcoder.jp/contests/dp/submissions/12268856)
* [N - Slimes](https://atcoder.jp/contests/dp/tasks/dp_n): First solution that came to mind was wine problem style DP however it will fail for testcase 2 and that's because in wine problem merging happens at ends \(connected\) here any two can pair up.

```text
Consider like a binary tree
10    10    10    10    10
 \   /      /
  \ /      /
  20,20   /
    \    /
     \  /

We basically have to compute such binary tree arrangement giving minimal result
let DP[l][r] denote minimal result from range l to r.
If l = r then it's simply 0
otherwise we have to merge and let a point x (x is l...r-1) then
DP[l][r] = min for all x: DP[l][x] + DP[x+1][r] + sum(l...r)
```
- Topdown [https://atcoder.jp/contests/dp/submissions/17518465](https://atcoder.jp/contests/dp/submissions/17518465)
- Bottomup [https://atcoder.jp/contests/dp/submissions/12226101](https://atcoder.jp/contests/dp/submissions/12226101)

* [S - Digit Sum](https://atcoder.jp/contests/dp/tasks/dp_s): Since our K is super large we cannot use it we have to store it in string and iterate over its digit. At a particular point if we cannot choose a digit bigger that the same moment digit of K \(unless we have encountered a smaller already before\). If we create a DP\[sum\]\[smallerAlready\] we can find count of numbers discovered so far \(while iterating digits\). In the end our answer will be at DP\[d\]\[false\] + DP\[d\]\[true\] but this denotes from 0 to K \(since we still iterate dig from 0 to 9 for 1st num\) so we have to add extra -1 in answer. [https://atcoder.jp/contests/dp/submissions/12245183](https://atcoder.jp/contests/dp/submissions/12245183)
* [O - Matching](https://atcoder.jp/contests/dp/tasks/dp_o): N is very low here so we are going to use bitmasking DP here. let's incremently pair every man with a woman and represent already paired woman in bitmask. Initially none are paired so DP\[0\] = 1 then we will find next state basically pair popcount th man and for woman try with all N woman first check if that one is currently single then add prevCount to newMask. [https://atcoder.jp/contests/dp/submissions/12251462](https://atcoder.jp/contests/dp/submissions/12251462)
* [J - Sushi](https://atcoder.jp/contests/dp/tasks/dp_j): At a particular scenerio sequence doesn't matter only number of plates having 1 sushi 2 sushi... matters so that's what will be represented in our state. Initial state is initial count from there we will move to other states. Keep a, b, c for loop order in mind. We find expected value assuming we reach that stage and then adding expValueWaste is without that assumption. [https://atcoder.jp/contests/dp/submissions/8751646](https://atcoder.jp/contests/dp/submissions/8751646)
* [U - Grouping](https://atcoder.jp/contests/dp/tasks/dp_u): Again a DP with bitmask problem, 2^n states which will represent if we picked those bit position best possible score. Starting from 0 state we first find notTaken positions and then iterate over that set picking element or not picking them. This last part although looks complex is still 3^n because there are only 3 possibilities for every item either taken, already taken, not taken. [https://atcoder.jp/contests/dp/submissions/12278381](https://atcoder.jp/contests/dp/submissions/12278381) Precomputation takes: O\(2^n \* n^2\) Rest takes: O\(2^n \* n  +  3^n\) Overall: O\(2^n \* n^2  + 3^n\)
* [Z - Frog 3](https://atcoder.jp/contests/dp/tasks/dp_z): O\(N^2\) solution simmilar to Frog 2 wont work due to constraint. \[Explanation below in convex hull part\] [https://atcoder.jp/contests/dp/submissions/12284255](https://atcoder.jp/contests/dp/submissions/12284255) \[Convex hull trick\] [https://atcoder.jp/contests/dp/submissions/12286413](https://atcoder.jp/contests/dp/submissions/12286413) \[Using Li Chao Tree\]
* X
* T
* V
* W

## Optimizations

### Convex Hull Trick

$$
DP[i] = min_{j < i}(DP[j] + b[j] * a[i])
$$

* Complexity O\(n^2\)  -&gt; O\(n\)
* Condition: b\[i + 1\] &lt;= b\[i\]

Problem: [https://atcoder.jp/contests/dp/tasks/dp\_z](https://atcoder.jp/contests/dp/tasks/dp_z)

```cpp
/* DP[i] = min{ DP[j] + (h[j] - h[i])^2 } + C
DP[i] = min{ DP[j] + h[j]^2 - 2h[i]h[j] } + h[i]^2 + C

min terms are of above form DP[j] + h[j]^2 is const term while
-2h[i]h[j] depends on h[i]. Also given h is increasing

Idea is to put a line Ax + B where A = -2h[i] and B = dp[i] + h[i]^2 making val
Ax + B at some point h[j] equal to our required minimized expression.
Start denotes initial line which hasn't been removed yet. If first line is >=
second line then we will increment start.

Final while loop is most important part of code.
If a inters b <= b inters line then popback where a is second last line and b
is last line */
struct Line
{
    int a, b;
    int value(int x) { return (a*x + b); }
    pii intersection(Line& other)
    {
        int p = other.b - b, q = a - other.a;
        if (q < 0) p *= -1, q *= -1;
        return {p, q};
    }
};
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, c; cin >> n >> c;
    vec<1, int> h(n);
    for (auto &x : h) cin >> x;

    vec<1, Line> lines;
    int start = 0;
    for (int i = 0; i < n; ++i)
    {
        while (start <= (int)lines.size() - 2)
        {
            Line a = lines[start], b = lines[start+1];
            if (a.value(h[i]) >= b.value(h[i])) ++start;
            else break;
        }

        int dp = (i == 0) ? 0 : lines[start].value(h[i]) + h[i]*h[i] + c;

        Line line{ -2*h[i], dp + h[i]*h[i] };
        while (start <= (int)lines.size() - 2)
        {
            Line a = lines.end()[-2], b = lines.end()[-1];
            pii x = a.intersection(b), y = b.intersection(line);
            if (x.F*y.S >= x.S*y.F) lines.pop_back();
            else break;
        }
        lines.push_back(line);

        if (i == n-1) cout << dp << '\n';
    }
    return 0;
}
```

### Li Chao tree <a id="toc-tgt-1"></a>

```cpp
const int MAXN = 2e7;
typedef pii point;
point line[4*MAXN];
int dot(point a, point b) { return (a.F*b.F + a.S*b.S); }
int f(point a, int x) { return dot(a, {x, 1}); }
void addLine(point pt, int v = 1, int l = 0, int r = MAXN)
{
    int m = (l+r) / 2;
    bool lef = (f(pt, l) < f(line[v], l)), mid = (f(pt, m) < f(line[v], m));
    if (mid) swap(line[v], pt);
    if (r-l == 1) return;
    else if (lef != mid) addLine(pt, 2*v, l, m);
    else addLine(pt, 2*v + 1, m, r);
}
int get(int x, int v = 1, int l = 0, int r = MAXN)
{
    int m = (l+r) / 2;
    if (r-l == 1) return f(line[v], x);
    else if (x < m) return min(f(line[v], x), get(x, 2*v, l, m));
    else return min(f(line[v], x), get(x, 2*v + 1, m, r));
}
```

Li Chao Implementation of above problem

```cpp
for (int i = 0; i < n; ++i)
{
    int dp = (i == 0) ? 0 : get(h[i]) + h[i]*h[i] + c;
    addLine({-2*h[i], dp + h[i]*h[i]});
    if (i == n-1) cout << dp << '\n';
}
```

### Divide & Conquer Optimization

### Knuth Optimization

## Non Trivial Problems

* [Boredom](https://codeforces.com/problemset/problem/455/A) - Given an array of n elements. You can pick a number ak and remove it \(only one of it's value instance\) along with all of elements of value ak+1 and ak-1. Doing so you will get ak points, find maximum number of points you can get. **Solution:** Consider frequencies and then traverse across all values. dp\[i\] = max \(dp\[i-1\], count of i \* i + dp\[i-2\]\) If we try finding optimal answer till ith element we can eit
* her remove that element or pick i-1th. [https://codeforces.com/contest/455/submission/72575905](https://codeforces.com/contest/455/submission/72575905)
* [K-Tree](https://codeforces.com/problemset/problem/431/C) - Given a k-nary tree having paths to child of weight 1, 2, 3...k. We want to find number of paths such that it's sum is n and contains atleast one edge of weight atleast d. **Solution:** Write a recursive solution and then memoize it. [https://codeforces.com/contest/431/submission/72577677](https://codeforces.com/contest/431/submission/72577677)
* [Hard Problem](https://codeforces.com/problemset/problem/706/C) - Given n strings and also an array of n integers. ith element in array denotes cost to reverse ith string. We want to arrange strings in lexiographical order in least cost possible. **Solution:** Create a pair of elements dp, first denotes minimal till that point if did not took reverse of ith string while second denotes if we took reverse. There will be four cases for every ith element those are - str\[i\] &gt;= str\[i-1\], &gt;= rev\_str\[i-1\], rev\_str\[i\] &gt;= str\[i-1\], &gt;= rev\_str\[i-1\] [https://codeforces.com/contest/706/submission/72579029](https://codeforces.com/contest/706/submission/72579029)
* [Another Problem On Strings](https://codeforces.com/problemset/problem/165/C) - Given a binary string an a value k, we have to find how many substrings from the given binary string have exactly k 1s in it. **Solution:** We just want to find subarray of sum k, use hashing very simple. [https://codeforces.com/contest/165/submission/72581785](https://codeforces.com/contest/165/submission/72581785)
* [Obtain The String](https://codeforces.com/problemset/problem/1295/C) - Given several testcases each having two strings s & t. We want to make t starting from an empty string by appending only subsequences of s. How many minimum subsequences we have to append. **Solution:** Store positions of each char in s as vector&lt;int&gt; idea is to iterate t and for each char in t we will find upper bound location of same char from previous chosen index. If we can't find such we will increment ans. Using set instead of array gives TLE. [https://codeforces.com/contest/1295/submission/72583393](https://codeforces.com/contest/1295/submission/72583393)
* [Chain Reaction](https://codeforces.com/problemset/problem/607/A) - Given an array of pairs. First denotes coordinate in 1D space, second denotes capacity of that bomb. When ith position bomb is activated it destroys all to its left which are capacity distance away \(inclusive\). We can place a new bomb strictly to the right of any existing one, find minimum no. of bombs that can be destroyed. **Solution:** We can create a dp considering values so dp\[i\] will mean we are considering bombs till ith position what is the maximum no. of bombs that can be destroyed. If we find max then minimum can be found easily by n-mx. Our dp relation will be dp\[i\] = dp\[i - x -1\] + 1. In case i has a valid bomb, x is it's capacity. Otherwise dp\[i\] = dp\[i-1\] [https://codeforces.com/contest/607/submission/72680942](https://codeforces.com/contest/607/submission/72680942)
* [Two Arrays](https://codeforces.com/problemset/problem/1288/C) - Given two numbers n \(&lt;= 1000\) & m \(&lt;= 10\). We have two tell how many combinations of 2 arrays are possible such that a is sorted in non decreasing while b is in non increasing and a\[i\] &lt;= b\[i\]. Size of array should be m and can only contains element b/w 1 to n \(repetition allowed\) **Solution:** Started with a recursive solution which picks current index element for both array basically itterating over all possibilities. To avoid TLE ise memoization. Now naive memoization is \[last0\]\[last1\]\[cur\] but it can be seen that overlapping subproblem holds for \[last1-last0\]\[cur\] so take that. Now observing answers it resolves down to a single formula after looking over OEIS. [https://codeforces.com/contest/1288/submission/72880919](https://codeforces.com/contest/1288/submission/72880919)
* [Dima and a Bad XOR](https://codeforces.com/problemset/problem/1151/B) - Given a matrix of n rows and m columns. We can pick one element from each row and keep xoring them in the end we want &gt; 0 value is it possible? if yes then print sequence of valid picking. **Solution:** My first approach was to create a 2D DP in which dp\[i\]\[j\] represents xor after picking mat\[i\]\[j\] as last value but it gave WA on a trivial case in which xor becomes zero and then can become greater again so doing max is wrong approach. [https://codeforces.com/contest/1151/submission/72972141](https://codeforces.com/contest/1151/submission/72972141) Correct approach is to consider a 2d dp of width 1024 \(as in consider all possible xors we can get\) [https://codeforces.com/contest/1151/submission/72979659](https://codeforces.com/contest/1151/submission/72979659)
* [RGB Substring](https://codeforces.com/problemset/problem/1196/D2) - Given a string of n length containing only RGB characters we want to convert it into a string by changing minimum number of characters such that a substring of modified string having length k is also a substring of RGBRGBRGBRGB... **Solution:** Naive approach which will work in easier version is do 3\*N\*K [https://codeforces.com/contest/1196/submission/73019555](https://codeforces.com/contest/1196/submission/73019555) Better solution is trying matching with pattern in sliding window fashion [https://codeforces.com/contest/1196/submission/73020056](https://codeforces.com/contest/1196/submission/73020056)
* [Star Sky](https://codeforces.com/problemset/problem/835/C) - Prepare a count dp such that we can find in O\(1\) count of particular color value star. Look at constraints carefully this problem is easy after looking that. [https://codeforces.com/problemset/problem/835/C](https://codeforces.com/problemset/problem/835/C)
* [Alyona and Spreadsheet](https://codeforces.com/problemset/problem/777/C) - Given a matrix with 10^5 rows and columns and 10^5 queries giving l & r. In each query we have to tell if we take l-r rows and ignore rest is there atleast 1 column which is sorted non decreasingly. **Solution:** Create a 2D dp marking the longest contiguous sorted column wise. To make it query in logN sort them and use lower bound. [https://codeforces.com/contest/777/submission/73358260](https://codeforces.com/contest/777/submission/73358260)
* [Dasha and Passwords](https://codeforces.com/problemset/problem/761/C) - Preety straight forward recursive + memoization type problem [https://codeforces.com/contest/761/submission/73365985](https://codeforces.com/contest/761/submission/73365985)
* [Colorful Bricks](https://codeforces.com/problemset/problem/1081/C) - Given n, m, k. There's an array of n tiles which we want to paint from given m colors such that exactly k tiles are painted differently from \(i-1\)th tile find number of such tiles possible. **Solution:** Create a 2D DP where dp\[n\]\[k\] is count if we have n tiles and k are different. dp\[n\]\[k\] = dp\[n\]\[k-1\] + dp\[n-1\]\[k-1\]\*\(m-1\) [https://codeforces.com/contest/1081/submission/73431055](https://codeforces.com/contest/1081/submission/73431055)
* [An impassioned circulation of affection](https://codeforces.com/problemset/problem/814/C) - Given a string containing lowercase letters say "kayomi" \(n &lt;= 1500\) next we will be given q queries in each we will be given m & ch. we have to tell max length subsegment of ch if we can repaint m cells. **Solution:** Let's consider we have to find max subsegment of o in "kayomi" we can create a 2D DP\[m+1\]\[n\] where dp\[i\]\[j\] represents maximum subsegment ending at j and we have done i repaints. dp\[i\]\[j\] = dp\[i-1\]\[j-1\] +1 \(if cur j is not o\) dp\[i\]\[j-1\] + 1 \(otherwise\). Now we can't query n^2 inside q queries so instead make a 3D DP storing for every 26 chars. [https://codeforces.com/contest/814/submission/73473937](https://codeforces.com/contest/814/submission/73473937)
* [Caeser's Legion](https://codeforces.com/contest/118/problem/D) - Given n1 n2 k1 k2 we have to place n1 soldiers and n2 horsemen such that atmost k1 soldiers and k2 horsemen are adjacent. Find number of such possible arrangements. **Solution:** Consider a 2D DP suggesting we are placing n1 \(columns\) and n2 \(rows\) dp\[i\]\[j\] is number of ways to place i horsemen and j soldiers while keeping the property now we will observe for dp property to hold we will have to split our 2D dp to 3D dp where z axis will have 2 units. 0 means placement ending with horsement and 1 means placement ending with soldier. Now dp relation can be easily established. [https://codeforces.com/contest/118/submission/74261384](https://codeforces.com/contest/118/submission/74261384)
* [Knapsack for All Segments](https://atcoder.jp/contests/abc159/tasks/abc159_f) - We basically want to find for every L, R pair possible count of subsequences having sum s. **Solution:** Considering 1 based indexing, if there's a subsequence giving sum s then overall it will add upto l\*\(n-r+1\) more to the final result. Fixing r we get \(l1 + l2 + ...\) \* \(n-r+1\) so if we find out for a particular end point r what all start point exists which give subsequence s then we may find the answer. Preparing such DP where DP\[i\]\[j\] means count of left points which have a subsequence of sum s \(l-r subsequence\) so for j == arr\[i\] it's i \(since all left point can be included\) For \(j &gt; arr\[i\]\) it's DP\[k\]\[j\] summation where k is for every &lt; i point so maintaining a prefSum \(vertically across columns\) [https://atcoder.jp/contests/abc159/submissions/12423455](https://atcoder.jp/contests/abc159/submissions/12423455)

