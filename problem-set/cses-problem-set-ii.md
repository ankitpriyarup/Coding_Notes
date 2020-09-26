# CSES Problem Set - II

## Range Queries

### [Range Sum Queries I](https://cses.fi/problemset/task/1646/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    vec<1, int> arr(n+1);
    for (int i = 1; i <= n; ++i) { cin >> arr[i]; arr[i] += arr[i-1]; }
    while (q--)
    {
        int l, r; cin >> l >> r;
        cout << arr[r] - arr[l-1] << '\n';
    }
    return 0;
}
```

### [Range Minimum Queries I](https://cses.fi/problemset/task/1647/)

```cpp
const int MAXN = 2e5+5;
vec<2, int> sparseTable(MAXN, 40, -1);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    int k = log2(n);
    for (int i = 0; i < n; ++i) cin >> sparseTable[i][0];
    for (int j = 1; j <= k; j++)
        for (int i = 0; i + (1 << j) <= n; i++)
            sparseTable[i][j] = min(sparseTable[i][j-1], sparseTable[i + (1 << (j-1))][j-1]);
    while (q--)
    {
        int L, R; cin >> L >> R;
        L--, R--;
        int j = log2(R-L+1);
        cout << min(sparseTable[L][j], sparseTable[R - (1<<j) + 1][j]) << '\n';
    }
    return 0;
}
```

### [Range Sum Queries II](https://cses.fi/problemset/task/1648/)

```cpp
const int N = 2e5;
int BIT[N+5];
void update(int i, int val)
{
    while (i <= N)
    {
        BIT[i] += val;
        i += (i & -i);
    }
}
int query(int i)
{
    int sm = 0;
    while (i)
    {
        sm += BIT[i];
        i -= (i & -i);
    }
    return sm;
}
int query(int l, int r) { return query(r) - query(l-1); }

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    for (int i = 1; i <= n; ++i) { int x; cin >> x; update(i, x); }
    while (q--)
    {
        int a, b, c; cin >> a >> b >> c;
        if (a == 1)
        {
            int prev = query(b, b);
            update(b, c-prev);
        }
        else cout << query(b, c) << '\n';
    }
    return 0;
}
```

### [Range Minimum Queries II](https://cses.fi/problemset/task/1649/)

```cpp
const int MAXN = 1e6;
int segTree[2*MAXN], _n_;
void build(int n)
{
    _n_ = n;
    for (int i = _n_-1; i > 0; --i)
        segTree[i] = min(segTree[i<<1], segTree[i<<1|1]);
}
void update(int p, int val)
{
    for (segTree[p += _n_] = val; p > 1; p >>= 1)
        segTree[p>>1] = min(segTree[p], segTree[p^1]);
}
int query(int l, int r) // Interval [l, r)
{
    int res = (1LL<<61);
    for (l += _n_, r += _n_; l < r; l >>= 1, r >>= 1)
    {
        if (l&1) res = min(res, segTree[l++]);
        if (r&1) res = min(res, segTree[--r]);
    }
    return res;
}

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    for (int i = 0; i < n; ++i) cin >> segTree[n+i];
    build(n);
    while (q--)
    {
        int a, b, c; cin >> a >> b >> c;
        if (a == 1) update(b-1, c);
        else cout << query(b-1, c) << '\n';
    }
    return 0;
}
```

### [Range Xor Queries](https://cses.fi/problemset/task/1650/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    vec<1, int> arr(n+1, 0);    // prefix xors
    for (int i = 1; i <= n; ++i) { cin >> arr[i]; arr[i] ^= arr[i-1]; }
    while (q--)
    {
        int l, r; cin >> l >> r;
        cout << (arr[r]^arr[l-1]) << '\n';
    }
    return 0;
}
```

### [Range Update Queries](https://cses.fi/problemset/task/1651/)

```cpp
const int N = 2e5 + 5;
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

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    for (int i = 1; i <= n; ++i) { int x; cin >> x; updateRange(i, i, x); }
    while (q--)
    {
        int t; cin >> t;
        if (t == 1)
        {
            int a, b, u; cin >> a >> b >> u;
            updateRange(a, b, u);
        }
        else
        {
            int a; cin >> a;
            cout << queryRange(a, a) << '\n';
        }
    }
    return 0;
}
```



