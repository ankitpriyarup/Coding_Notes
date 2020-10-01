# Range Queries

## Prefix Sum
[https://leetcode.com/problems/range-sum-query-2d-immutable/](https://leetcode.com/problems/range-sum-query-2d-immutable/)
```c++
class NumMatrix {
public:
    vector<vector<int>> dp;
    NumMatrix(vector<vector<int>>& matrix)
    {
        if (matrix.empty()) return;
        int n = matrix.size(), m = matrix[0].size();
        dp = vector<vector<int>>(n+1, vector<int>(m+1, 0));
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                dp[i+1][j+1] = matrix[i][j] + dp[i][j+1] + dp[i+1][j] - dp[i][j];
    }
    
    int sumRegion(int row1, int col1, int row2, int col2)
    {
        return dp[row2+1][col2+1] - dp[row1][col2+1] - dp[row2+1][col1] + dp[row1][col1];
    }
};
```


## Prefix XOR

## Difference Array

## Sparse Table

Every positive integer can easily be represented as a sum of powers of 2 given by its decimal representation. Similarly, any interval \[l, r\] can be broken down into smaller intervals of powers of 2.

![](.gitbook/assets/image%20%28138%29.png)

Now imagine if we could precompute the range query answer for all those intervals and combine them. Precompute answers for all intervals of size 2^x

It can query in logarithmic time but for overlap friendly functions i.e. f\(f\(a, b\), f\(b, c\)\) = f\(a, f\(b, c\)\) it can do it in constant time.

![Highlighted ones are overlap friendly](.gitbook/assets/image%20%2863%29.png)

**Building:** dp\[i\]\[j\] = min of array from i to i + 2^j. j goes from 0 till floor\(log2\(n\)\)

![](.gitbook/assets/image%20%28122%29%20%281%29.png)

![](.gitbook/assets/image%20%2833%29.png)

![](.gitbook/assets/image%20%2860%29%20%281%29.png)

```cpp
int st[MAXN][K + 1];
// Build O(NlogN): here f is function of operation like sum or min
for (int i = 0; i < N; i++) st[i][0] = f(array[i]);
for (int j = 1; j <= K; j++)
    for (int i = 0; i + (1 << j) <= N; i++)
        st[i][j] = f(st[i][j-1], st[i + (1 << (j - 1))][j - 1]);

// Sum query: O(logN)
int sm = 0;
for (int j = p; j >= 0; --j)
    if ((1 << j) <= R-L+1) { sm += st[L][j]; L += (1<<j); }

// Minimum query: O(1)
int j = log2(R-L+1);
int mn = min(st[L][j], st[R - (1<<j) + 1][j]);
```

## Disjoint Sparse Table

How about O\(1\) query for all type of functions? \(and KlogK time build\) This isn't a dream baby XD  
[https://discuss.codechef.com/t/tutorial-disjoint-sparse-table/17404](https://discuss.codechef.com/t/tutorial-disjoint-sparse-table/17404)

```cpp
#define MAXN 1000000
#define MAXPOWN 1048576		// 2^(ceil(log_2(MAXN)))
#define MAXLVL 21			// ceil(log_2(MAXN)) + 1
#define INIT sz = n; maxLvl = __builtin_clz(n) ^ 31; if((1<<maxLvl) != n) sz = 1<<++maxLvl;
int n, p, maxLvl, sz, arr[MAXPOWN], sparseTable[MAXLVL][MAXPOWN];
inline int func(int x, int y) { return min(x, y); }
void build(int lvl = 0, int l = 0, int r = sz)
{
	int m = (l+r)/2;
	sparseTable[lvl][m] = arr[m]%p;
	for (int i = m-1; i >= l; --i) sparseTable[lvl][i] = func(sparseTable[lvl][i+1], arr[i])%p;
	if (m+1 < r)
	{
		sparseTable[lvl][m+1] = arr[m+1]%p;
		for (int i = m+2; i < r; ++i) sparseTable[lvl][i] = func(sparseTable[lvl][i-1], arr[i])%p;
	}
	if (l+1 != r) { build(lvl+1, l, m); build(lvl+1, m, r); }
}
int query(int L, int R)
{
	if (L == R) return arr[L]%p;
	int k = __builtin_clz(L^R) ^ 31;
	int lvl = maxLvl - 1 - k;
	int res = sparseTable[lvl][L];
	if (R & ((1<<k) - 1)) res = func(res, sparseTable[lvl][R])%p;
	return res;
}

signed main()
{
	ios_base::sync_with_stdio(false); cin.tie(NULL);
	p = 1e9 + 7;
	cin >> n;
	for (int i = 0; i < n; ++i) cin >> arr[i];
	INIT;
	build();
	int q; cin >> q;
	while (q--)
	{
		int l, r; cin >> l >> r;
		bug(l, r);
		cout << query(l, r) << '\n';
	}
	return 0;
}
```

## Fenwick Tree

It is used to calculate prefix sum of an array. It's not as powerful as Segment Tree but it's simpler to implement.  
 In the image, BIT will be stored in array of n+1 length. \(indexing starts with 1 because binary operation used later on will be anomoly for 0\) for each say 3 find 3 & -3 this will flip right most set bit giving the parent from tree.  
Now to fill all value say 4 we find 4 = 0 + 2^2 means we go from 0th index to next 4 elements or for 11 = 2^3 + 2^1 + 2^0 from 10-10 ![](res/BIT.png)  
 If we need to find sum from 0 to 5 we first go to value 6 take it here it's 9 then we go to it's parent it's 10 so ans is 19

![In BIT: say 4th element is responsible for 1-4 elements \(last set bit\)](.gitbook/assets/image%20%2880%29.png)

```cpp
void update(int BIT[], int n, int i, int incr)
{
    while (i <= n)
    {
        BIT[i] += incr;
        i += (i & -i);
    }
}
int query(int BIT[], int n, int i)
{
    int ans = 0;
    while (i > 0)
    {
        ans += BIT[i];
        i -= (i & -i);
    }
    return ans;
}
```

### Binary Search in BIT

Given a data of heights: \[1, 8\] \[2, 2\] \[3, 10\] \[4, 100\] \[5, 1\] \[6, 2\] Basically 8 peoples are of height 1, 2 of height 2, 10 of height 3 and so on. We can query to determine the kth tallest person. Or we can update number of people count for a particular height.

![](.gitbook/assets/image%20%28143%29.png)

Approach \#1: For query we can apply binary search, so check middle height say 3 then find it's count using fenwick tree and so on. It will be O\(nlogn\)

Approach \#2: Given below is how the fenwick tree will look like. Start off with the highest bit on which is 16 then check middle which can be find by right shifting again check refer diagram. O\(logn\)

![](.gitbook/assets/image%20%2843%29%20%281%29.png)

### Above the Median

Given an array representing height of ith member. We need find count of possible subarrays such that there median \(a\[ceil\(n/2\)\]\) is greater than or equal to x.

Example: \[10, 5, 6, 2\] x = 6  
Answer is: 7 =&gt; {10}, {6}, {10, 5}, {5, 6}, {6, 2}, {10, 5, 6}, {10, 5, 6, 2}.

We can replace element in arr with 1 if it's &gt;= x and -1 if it's less  
\[1, -1, 1, -1\]  
Now our problem is reduced to finding subarrays with non-negative sum.  
Calculate prefix sum  
\[1, 0, 1, 0\]  
Now we basically want to find L & R such that p\[R\] - p\[L\] is &gt;= 0 this simply reduces down to inversion count problem.

### Read Single Element

* Simply do BIT\[i\] - BIT\[i-1\] but it will be 2logn Time
* Maintain an array storing actual elements as well N space
* Third way is based on the fact that when we do BIT\[i\] - BIT\[i-1\] both i and i-1 will eventually converge at a point which may be zero

```cpp
int readSingle(int i)
{
    int sum = BIT[i];
    if (i > 0)
    {
        int z = i - (i & -i);
        i--;
        while (i != z) sum -= BIT[i], i -= (i & -i);
    }
    return sum;
}
```

### Find index with given cumulative frequency

Naive way is to iterate keep having cumulativeFreq in logN and check linearly which will be Nlogn. In case of negative  frequencies that's the only way. In non-negative scenario we can use binary search modification making it just logN.

```cpp
int find(int cumFreq, int n)
{
    int i = 0;
    int bitMask = (int)(pow(2, (int)(log2(n))));
    while (bitMask)
    {
        int temp = i + bitMask;
        bitMask >>= 1;
        if (temp > n) continue;
        if (cumFreq == BIT[temp]) return temp;
        else if (cumFreq > BIT[temp]) i = temp, cumFreq -= BIT[temp];
    }
    if (cumFreq != 0) return -1;
    else return i;
}
```

### 2D BIT

Say you have a plane with dots \(with non-negative coordinates\) There are three queries.

* Set a dot at \(x, y\)
* Remove the dot from \(x, y\)
* Count the number of dots in rectangle \(0, 0\) \(x, y\) - \(down-left and top-right corner\)

```cpp
void update(int x, int y, int val)
{
    while (x <= MAX_X)
    {
        int _y = y;
        while (_y <= MAX_Y)
            BIT[x][_y] += val, _y += (_y & -_y);
        x += (x & -x);
    }
}
void query(int x, int y)
{
    int sum = 0;
    while (x)
    {
        int _y = y;
        while (_y)
            sum += BIT[x][_y], _y -= (_y & -_y);
        x -= (x & -x);
    }
    return sum;
}
```

### Range Update & Range Query

```cpp
const int N = 1005;
vector<int> BIT1(N), BIT2(N);
void update(vector<int> &BIT, int i, int val)
{
    while (i <= N)
    {
        BIT[i] += val;
        i += (i & -i);
    }
}
int query(vector<int> &BIT, int i)
{
    int sm = 0;
    while (i)
    {
        sm += BIT[i];
        i -= (i & -i);
    }
    return sm;
}
int summation(int x) { return ((query(BIT1, x)*x) - query(BIT2, x)); }  // sum of [1, x]
int queryRange(int l, int r) { return summation(r) - summation(l-1); }  // query [l, r]
void updateRange(int l, int r, int val)                                 // update [l, r]
{
    update(BIT1, l, val); update(BIT1, r+1, -val);
    update(BIT2, l, val*(l-1)); update(BIT2, r+1, -val*r);
}
```

## Wavelet Trees

```cpp
#undef int
const int MAXN = 3e5 + 10;
const int MX = 1e6;
int arr[MAXN];
struct wavelet_tree
{
private:
    int lo, hi;
    wavelet_tree *l, *r;
    vec<1, int> b;

public:
    wavelet_tree(int *from, int *to, int x, int y)
    {
        lo = x, hi = y;
        if (lo == hi || from >= to) return;
        int mid = (lo + hi)/2;
        auto f = [mid](int x){ return x <= mid; };
        b.reserve(to - from + 1); b.push_back(0);
        for (auto it = from; it != to; it++) b.push_back(b.back() + f(*it));
        auto pivot = stable_partition(from, to, f);
        l = new wavelet_tree(from, pivot, lo, mid);
        r = new wavelet_tree(pivot, to, mid+1, hi);
    }

    ~wavelet_tree()
    {
        delete l;
        delete r;
    }

    int kthSmallest(int l, int r, int k)    // [l, r]
    {
        if (l > r) return 0;
        if (lo == hi) return lo;
        int inLeft = b[r] - b[l-1], lb = b[l-1], rb = b[r];
        if (k <= inLeft) return this->l->kthSmallest(lb+1, rb, k);
        return this->r->kthSmallest(l-lb, r-rb, k-inLeft);
    }

    int lessThanEqualToK(int l, int r, int k)    // [l, r]
    {
        if (l > r or k < lo) return 0;
        if (hi <= k) return r - l + 1;
        int lb = b[l-1], rb = b[r];
        return this->l->lessThanEqualToK(lb+1, rb, k) + this->r->lessThanEqualToK(l-lb, r-rb, k);
    }

    int equalToK(int l, int r, int k)    // [l, r]
    {
        if (l > r or k < lo or k > hi) return 0;
        if (lo == hi) return r - l + 1;
        int lb = b[l-1], rb = b[r], mid = (lo+hi)/2;
        if (k <= mid) return this->l->equalToK(lb+1, rb, k);
        return this->r->equalToK(l-lb, r-rb, k);
    }
};
/*
    // CODE
    int n; cin >> n;
    for (int i = 1; i <= n; ++i) cin >> arr[i];
    wavelet_tree T(arr+1, arr+n+1, 1, MX);
    int q; cin >> q;
    while (q--)
    {
        int t, l, r, k; cin >> t >> l >> r >> k;
        if (t == 1) cout << "kthSmallest [" << l << "," << r << "]: " << T.kthSmallest(l, r, k) << '\n';
        else if (t == 2) cout << "lessThanEqualToK [" << l << "," << r << "]: " << T.lessThanEqualToK(l, r, k) << '\n';
        else if (t == 3) cout << "equalToK [" << l << "," << r << "]: " << T.equalToK(l, r, k) << '\n';
    }

    // INPUT -> OUTPUT
    7
    1 2 3 3 5 4 2                   
    4
    1 5 7 1                     1thSmallest [5,7]: 2
    1 5 7 2                     2thSmallest [5,7]: 4
    2 2 6 4                     lessThanEqualTo4 [2,6]: 4
    3 2 5 3                     equalTo3 [2,5]: 2
*/
```

## Segment Tree

## Square Root Decomposition

