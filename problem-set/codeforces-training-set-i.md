# Training Set I

## Day 1 \(9\)

### [632C - The Smallest String Concatenation](https://codeforces.com/problemset/problem/632/C)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, string> arr(n);
    for (auto &x : arr) cin >> x;
    sort(all(arr), [](string &x, string &y){ return (x+y < y+x); });
    string res = "";
    for (auto &x : arr) res += x;
    cout << res << '\n';
    return 0;
}
```

### [922D - Robot Vacuum Cleaner](https://codeforces.com/problemset/problem/922/D)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, string> arr(n);
    for (auto &x : arr) cin >> x;
    auto f = [](string s)
    {
        int noise = 0, x = 0;
        for (auto &ch : s)
        {
            if (ch == 's') x++;
            else noise += x;
        }
        return noise;
    };
    sort(all(arr), [&](string &x, string &y){ return f(x+y) > f(y+x); });
    string res = "";
    for (auto &x : arr) res += x;
    cout << f(res) << '\n';
    return 0;
}
```

### [489D - Unbearable Controversy of Being](https://codeforces.com/problemset/problem/489/D)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, int> adj[n+1];
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
    }
    int res = 0;
    intHashTable cnts;
    for (int i = 1; i <= n; ++i)
    {
        cnts.clear();
        for (auto &x : adj[i])
            for (auto &y : adj[x]) cnts[y]++;
        for (auto &x : cnts)
        {
            if (x.F == i) continue;
            res += (x.S * (x.S-1)) / 2;    // x.S choose 2
        }
    }
    cout << res << '\n';
    return 0;
}
```

### [187B - AlgoRace](https://codeforces.com/problemset/problem/187/B)

```cpp
const int MAXN = 60+3;
int mat[MAXN][MAXN][MAXN], dp[MAXN][MAXN][MAXN], res[MAXN][MAXN][MAXN], cur[MAXN][MAXN];
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, k, q; cin >> n >> k >> q;
    // dp stores if we choose this car then what is shortest time using floyd warshall
    for (int w = 0; w < k; ++w)
    {
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j) { cin >> mat[w][i][j]; dp[w][i][j] = mat[w][i][j]; }
        for (int x = 0; x < n; ++x)
            for (int i = 0; i < n; ++i)
                for (int j = 0; j < n; ++j)
                    dp[w][i][j] = min(dp[w][i][j], dp[w][i][x] + dp[w][x][j]);
    }
    memset(res, 63, sizeof(res));
    memset(cur, 63, sizeof(cur));
    // cur stores shortest time without changing car
    for (int i = 0; i < n; ++i)
    {
        for (int j = 0; j < n; ++j)
        {
            for (int x = 0; x < k; ++x) cur[i][j] = min(cur[i][j], dp[x][i][j]);
            res[0][i][j] = cur[i][j];
        }
    }
    for (int w = 1; w < MAXN; ++w)
    {
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                for (int x = 0; x < n; ++x)
                    res[w][i][j] = min(res[w][i][j], cur[i][x] + res[w-1][x][j]);
    }
    while (q--)
    {
        int u, v, t; cin >> u >> v >> t;
        cout << res[min(t, n)][u-1][v-1] << '\n';
    }
    return 0;
}
```

### [1249E - By Elevator or Stairs?](https://codeforces.com/problemset/problem/1249/E)

```cpp
vec<2, int> dp(200005, 2, (1<<30));
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, c; cin >> n >> c;
    vec<1, int> a(n), b(n);
    for (int i = 1; i < n; ++i) cin >> a[i];
    for (int i = 1; i < n; ++i) cin >> b[i];
    dp[1][0] = 0;
    for (int i = 1; i < n; ++i)
    {
        for (bool onLift : {false, true})
        {
            dp[i+1][false] = min(dp[i+1][false], a[i] + dp[i][onLift]);
            dp[i+1][true] = min(dp[i+1][true], b[i] + dp[i][onLift] + (onLift ? 0 : c));
        }
    }
    for (int i = 1; i <= n; ++i) cout << min(dp[i][0], dp[i][1]) << " ";
    cout << '\n';
    return 0;
}
```

### [813C - The Tag Game](https://codeforces.com/problemset/problem/813/C)

```cpp
const int MAXN = 2e5+5;
#define INF (1LL<<61)
vec<1, int> adj[MAXN];
vec<1, int> dist1(MAXN, INF), dist2(MAXN, INF);
void dfs(int u, vec<1, int> &dist, int prev = -1, int cur = 0)
{
    dist[u] = min(dist[u], cur);
    for (auto &v : adj[u])
        if (v != prev) dfs(v, dist, u, cur+1);
}
void dfs2(int u, int &res, int &pt, int prev = -1)
{
    if (dist1[u] <= dist2[u]) return;
    if (dist1[u] > res) res = dist1[u], pt = u;
    for (auto &v : adj[u])
        if (v != prev) dfs2(v, res, pt, u);
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, x; cin >> n >> x;
    int m = n-1;
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, dist1);
    dfs(x, dist2);
    int res = 0, pt = -1;
    dfs2(x, res, pt);
    cout << 2*max(dist1[pt], dist2[pt]) << '\n';
    return 0;
}
```

### [946D - Timetable](https://codeforces.com/contest/946/problem/D)

dp states would be cur pos and number of skips. On a day we have to choose an optimal substring doing so in N^2 gives TLE. Now the idea is to construct some intermediary dp hours\[i\]\[j\] denotes minimum no of hours on its day with j skips we can find so by sliding window concept now it is linear we basically utilized an already present dp state \(skips\) to optimally find hours.

```cpp
// TLE Version
int n, m, k;
vec<1, int> arr[505];
vec<2, int> dp(505, 505, -1);
int solve(int cur = 0, int skipped = 0)
{
    if (skipped > k) return (1LL<<61);
    if (cur == n) return 0;
    if (dp[cur][skipped] != -1) return dp[cur][skipped];
 
    int res = solve(cur+1, skipped+sz(arr[cur]));
    for (int l = 0; l < sz(arr[cur]); ++l)
    {
        for (int r = l; r < sz(arr[cur]); ++r)
        {
            int alreadyWasted = (arr[cur][r]-arr[cur][l]+1);
            if (alreadyWasted > res) continue;
            res = min(res, alreadyWasted + solve(cur+1, skipped+(sz(arr[cur])-(r-l+1))));
        }
    }
    return dp[cur][skipped] = res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    cin >> n >> m >> k;
    for (int i = 0; i < n; ++i)
    {
        string str; cin >> str;
        for (int j = 0; j < sz(str); ++j)
            if (str[j] == '1') arr[i].push_back(j);
    }
    cout << solve() << '\n';
    return 0;
}

// AC Version
const int MAX = 510;
string s[MAX];
int hours[MAX][MAX];    // minimum no of hrs on ith day with j skips
int dp[MAX][MAX];       // minimum no of hrs upto ith day with j skips
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int days, hrs, skips; cin >> days >> hrs >> skips;
    for (int i = 1; i <= days; i++) cin >> s[i];
    for (int i = 1; i <= days; i++)
    {
        int lesson_hrs = 0;
        for (auto c : s[i])
            if (c == '1') lesson_hrs++;
        for (int j = 0; j <= skips; j++)
        {
            int temp = max(0LL, lesson_hrs - j);
            if (temp == 0) { hours[i][j] = 0; continue; }
            // find minimum period to attend temp no of lessons using sliding window
            int mn = MAX, cnt = 0, idx = -1;
            for (int k = 0; k < hrs; k++)
            {
                if (s[i][k] == '0') continue;
                while (cnt < temp && idx + 1 < hrs)
                {
                    idx++;
                    if (s[i][idx] == '1') cnt++;
                }
                if (cnt == temp) mn = min(mn, idx - k + 1);
                cnt--;
            }
            hours[i][j] = mn;
        }
    }

    dp[0][0] = 0;
    for (int i = 1; i < MAX; i++) dp[0][i] = MAX * MAX;
    for (int i = 1; i <= days; i++)
    {
        for (int j = 0; j <= skips; j++)
        {
            dp[i][j] = MAX * MAX;
            for (int k = 0; k <= j; k++) dp[i][j] = min(dp[i][j], dp[i-1][j-k] + hours[i][k]);
        }
    }
    cout << dp[days][skips] << endl;
    return 0;
}
```

### [909C - Python Indentation](https://codeforces.com/problemset/problem/909/C)

```cpp
vec<2, int> dp(5005, 5005, 0);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    string str = "";
    for (int i = 0; i < n; ++i) { char ch; cin >> ch; str += ch; }
    dp[0][0] += 1, dp[0][1] -= 1;
    for (int i = 0; i < n; ++i)
    {
        for (int j = 1; j < n; ++j) (dp[i][j] += dp[i][j-1]) %= MOD;
        for (int j = 0; j < n; ++j)
        {
            if (dp[i][j] == 0) continue;
            if (str[i] == 'f') (dp[i+1][j+1] += dp[i][j]) %= MOD, (dp[i+1][j+2] += MOD-dp[i][j]) %= MOD;
            else (dp[i+1][0] += dp[i][j]) %= MOD, (dp[i+1][j+1] += MOD-dp[i][j]) %= MOD;
        }
    }
    int res = 0;
    for (int j = 0; j < n; ++j) (res += dp[n-1][j]) %= MOD;
    cout << res << '\n';
    return 0;
}
```

### [766C - Mahmoud and a Message](https://codeforces.com/contest/766/problem/C)

```cpp
vec<1, int> arr(26);
#define INF (1<<30)
vec<2, int> dp(1005, 3, 0);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    string str; cin >> str;
    for (auto &x : arr) cin >> x;
    dp[0][0] = 1; dp[0][2] = 0;
    for (int i = 1; i <= n; ++i) dp[i][2] = INF;
    for (int i = 0; i < n; ++i)
    {
        for (int j = i+1, mn = INF; j <= n; ++j)
        {
            mn = min(mn, arr[str[j-1] - 'a']);
            if (j-i > mn) break;
            (dp[j][0] += dp[i][0]) %= MOD;
            dp[j][1] = max({dp[j][1], dp[i][1], j-i});
            dp[j][2] = min(dp[j][2], dp[i][2]+1);
        }
    }
    cout << dp[n][0] << '\n' << dp[n][1] << '\n' << dp[n][2] << '\n';
    return 0;
}
```

## Day 2 \(8\)

### [827A - String Reconstruction](https://codeforces.com/problemset/problem/827/A)

```cpp
vec<1, char> arr(2000005, 'a');
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    int mx = 0;
    for (int i = 0; i < n; ++i)
    {
        string s; cin >> s;
        int sz; cin >> sz;
        int ptr = 1;
        while (sz--)
        {
            int x; cin >> x;
            for (int j = max(x, ptr); j < x+sz(s); ++j) arr[j] = s[j-x];
            ptr = x+sz(s)-1;
        }
        mx = max(mx, ptr);
    }
    for (int i = 1; i <= mx; ++i) cout << arr[i];
    cout << '\n';
    return 0;
}
```

### [1290B - Irreducible Anagrams](https://codeforces.com/problemset/problem/1290/B)

```cpp
/* Its length is equal to 1.
Its first and last characters are different.
It contains at least three different characters. */
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    string str; cin >> str;
    int q; cin >> q;
    vec<2, int> cnt(sz(str)+1, 26, 0);
    for (int i = 1; i <= sz(str); ++i)
    {
        for (int j = 0; j < 26; ++j) cnt[i][j] = cnt[i-1][j];
        cnt[i][str[i-1]-'a']++;
    }
    while (q--)
    {
        int l, r; cin >> l >> r;
        if (l == r || str[l-1] != str[r-1]) cout << "Yes\n";
        else
        {
            int unique = 0;
            for (int j = 0; j < 26; ++j)
                if ((cnt[r][j] - cnt[l][j]) > 0) unique++;
            if (unique >= 3) cout << "Yes\n";
            else cout << "No\n";
        }
    }
    return 0;
}
```

### [580D - Kefa and Dishes](https://codeforces.com/contest/580/problem/D)

Nice dp with bit masking

```cpp
vec<1, int> arr(20);
vec<2, int> adj(20, 20, 0);
vec<2, int> dp((1<<20), 20, -1);
int n, m, k;
int solve(int state, int u)
{
    if (__builtin_popcount(state) == m) return 0;
    if (dp[state][u] != -1) return dp[state][u];
    int res = 0;
    for (int v = 0; v < n; ++v)
        if (!(state&(1<<v))) res = max(res, arr[v] + adj[u][v] + solve(state|(1<<v), v));
    return dp[state][u] = res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    cin >> n >> m >> k;
    for (int i = 0; i < n; ++i) cin >> arr[i];
    while (k--)
    {
        int u, v, w; cin >> u >> v >> w;
        adj[u-1][v-1] = w;
    }
    int ans = 0;
    for (int i = 0; i < n; ++i)
        ans = max(ans, arr[i] + solve(1<<i, i));
    cout << ans << '\n';
    return 0;
}
```

### [385C - Bear and Prime Numbers](https://codeforces.com/contest/385/problem/C)

```cpp
int MAXN = 1e7+5;
vec<1, int> freq(MAXN, 0), cnt(MAXN, 0);
vec<1, bool> isPrime(MAXN, true);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, int> arr(n);
    for (auto &x : arr) { cin >> x; freq[x]++; }
    for (int i = 2; i < MAXN; ++i)
    {
        if (!isPrime[i]) continue;
        for (int j = i; j < MAXN; j += i)
            cnt[i] += freq[j], isPrime[j] = false;
    }
    for (int i = 1; i < MAXN; ++i) cnt[i] += cnt[i-1];
    int q; cin >> q;
    while (q--)
    {
        int l, r; cin >> l >> r;
        l = min(l, MAXN-1);
        r = min(r, MAXN-1);
        cout << cnt[r]-cnt[l-1] << '\n';
    }
    return 0;
}
```

### [483B - Friends and Presents](https://codeforces.com/contest/483/problem/B)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int c1, c2, x, y; cin >> c1 >> c2 >> x >> y;

    auto check = [&](int mid)
    {
        if ((mid - mid/x) < c1) return false;               // not divisible by x >= c1
        if ((mid - mid/y) < c2) return false;               // not divisible by y >= c2
        if ((mid - mid/lcm(x, y)) < c1+c2) return false;    // not divisible by x or y >= c1+c2
        return true;
    };

    int l = 0, r = 1e18;
    while (l+1 < r)
    {
        int mid = l + (r-l)/2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    cout << r << '\n';
    return 0;
}
```

### [1096D - Easy Problem](https://codeforces.com/contest/1096/problem/D)

```cpp
const int MAXN = 1e5+5;
int n;
string str;
vec<1, int> arr(MAXN);
#define INF (1LL<<61)
string reqd = "hard";
vec<2, int> dp(MAXN, 5, -1);
int solve(int cur = 0, int end = 0)
{
    if (end == 4) return INF;
    if (cur == n) return 0;
    if (dp[cur][end] != -1) return dp[cur][end];

    return dp[cur][end] = min(arr[cur] + solve(cur+1, end), solve(cur+1, end + (reqd[end] == str[cur])));
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    cin >> n;
    cin >> str;
    for (int i = 0; i < n; ++i) cin >> arr[i];
    cout << solve() << '\n';
    return 0;
}
```

### [370C - Mittens](https://codeforces.com/problemset/problem/370/C)

```cpp
// Boring questions
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, int> arr(n);
    for (auto &x : arr) cin >> x;
    sort(all(arr));

    int ans = 0;
    vec<1, pii> res;
    for (int i = 0; i < n; ++i)
    {
        if (arr[i] != arr[(i + n/2)%n]) { ans++; res.push_back({arr[i], arr[(i + n/2)%n]}); }
        else res.push_back({arr[i], arr[i]});
    }
    cout << ans << '\n';
    for (auto &x : res)
        cout << x.F << " " << x.S << '\n';
    return 0;
}
```

### [1355C - Count Triangles](https://codeforces.com/contest/1355/problem/C)

```cpp
const int MAXN = 1000005;
vec<1, int> arr(MAXN);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int a, b, c, d; cin >> a >> b >> c >> d;
    for (int x = a; x <= b; ++x) arr[x+b]++, arr[x+c+1]--;
    for (int i = 1; i < MAXN; ++i) arr[i] += arr[i-1];
    for (int i = 1; i < MAXN; ++i) arr[i] += arr[i-1];
    int res = 0;
    for (int i = c; i <= d; ++i) res += arr[MAXN-1]-arr[i];
    cout << res << '\n';
    return 0;
}
```

## Day 3 \(5\)

### [1354D - Multiset](https://codeforces.com/contest/1354/problem/D)

Nice binary search \(Using pbds gives MLE\)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    vec<1, int> a(n), k(q);
    for (auto &x : a) cin >> x;
    for (auto &x : k) cin >> x;

    // counts elements <= mid after considering all queries
    auto cnt = [&](int mid) 
    {
        int res = 0;
        for (auto &x : a)
            if (x <= mid) res++;
        for (auto &x : k)
        {
            if (x > 0 && x <= mid) res++;
            if (x < 0 && abs(x) <= res) res--;
        }
        return res;
    };

    if (cnt(1e9) == 0) { cout << "0\n"; return 0; }
    int l = 0, r = 1e6 + 1;
    while (l+1 < r)
    {
        int mid = (l+r)/2;
        if (cnt(mid) > 0) r = mid;
        else l = mid;
    }
    cout << r << '\n';
    return 0;
}
```

### [1358D - The Best Vacation](https://codeforces.com/contest/1358/problem/D)

```cpp
const int sumUp(int n) { return (n*(n+1))/2; }
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, x; cin >> n >> x;
    vec<1, pii> arr(2*n);
    for (int i = 0; i < n; ++i)
    {
        cin >> arr[i].F;
        arr[i].S = sumUp(arr[i].F);
        arr[n+i] = arr[i];
    }

    set<pii, greater<pii>> rec;
    rec.insert({0, 0});
    int ans = 0;
    for (int i = 0, smF = 0, smS = 0; i < 2*n; ++i)
    {
        smF += arr[i].F, smS += arr[i].S;
        int reqd = smF-x, res;
        if (reqd > 0)
        {
            auto it = rec.lower_bound({reqd, 0});
            res = smS - it->second - sumUp(reqd - it->F);
        }
        else res = smS;
        ans = max(ans, res);
        rec.insert({smF, smS});
    }
    cout << ans << '\n';
    return 0;
}
```

### [1360F - Spy-string](https://codeforces.com/contest/1360/problem/F)

```cpp
int n, m;
vec<2, int> visited(15, (1<<15), false);
vec<1, char> res;
bool found = false;
void solve(vec<1, string> &words, int cur = 0, int state = 0)
{
    if (cur == m)
    {
        for (auto &x : res) cout << x; cout << '\n';
        found = true;
        return;
    }
    if (visited[cur][state]) return;
    visited[cur][state] = true;
    for (char ch = 'a'; ch <= 'z'; ++ch)
    {
        bool valid = true;
        int newState = state;
        for (int i = 0; i < n; ++i)
        {
            if (words[i][cur] != ch)
            {
                if (!(newState&(1<<i))) newState |= (1<<i);
                else { valid = false; break; }
            }
        }
        if (valid)
        {
            res.push_back(ch);
            solve(words, cur+1, newState);
            res.pop_back();
        }
        if (found) return;
    }
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        cin >> n >> m;
        vec<1, string> words(n);
        for (auto &x : words) cin >> x;
        for (auto &x : visited) for (auto &y : x) y = false;
        res.clear();
        found = false;
        solve(words);
        if (!found) cout << "-1\n";
    }
    return 0;
}
```

### [1360G - A/B Matrix](https://codeforces.com/contest/1360/problem/G)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int n, m, a, b; cin >> n >> m >> a >> b;
        if (n*a != m*b) cout << "NO\n";
        else
        {
            vec<2, int> res(n, m, 0);
            int shift = 0;
            for (shift = 1; shift < m; ++shift)
                if (shift*n % m == 0) break;
            for (int i = 0, dx = 0; i < n; ++i, dx += shift)
                for (int j = 0; j < a; ++j)
                    res[i][(j+dx) % m] = 1;
            
            cout << "YES\n";
            for (auto &x : res)
            {
                for (auto &y : x) cout << y;
                cout << '\n';
            }
        }
    }
    return 0;
}
```

### [1353D - Constructing the Array](https://codeforces.com/contest/1353/problem/D)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int n; cin >> n;
        set<pii> st;
        st.insert({n, -1});     // stores size of zeros and left pos
        int op = 1;
        vec<1, int> arr(n+1);
        while (!st.empty())
        {
            auto cur = *st.rbegin(); st.erase(cur);     // pick greatest size leftmost
            cur.S *= -1;
            int rightPos = (cur.S + cur.F - 1);
            int mid = (cur.S + rightPos)/2;
            arr[mid] = op++;
            if (mid - cur.S) st.insert({mid - cur.S, -cur.S});
            if (rightPos - mid) st.insert({rightPos - mid, -(mid+1)});
        }
        for (int i = 1; i <= n; ++i) cout << arr[i] << " "; cout << '\n';
    }
    return 0;
}
```

## Day 4 \(5\)

### [1350C - Orac and LCM](https://codeforces.com/contest/1350/problem/C)

We want to compute GCD\(LCM of all pairs\). If we denote a number as prime factorizations,  
say 12 = 2^2 \* 3^1 and 8 = 2^3 \* 3^0 then LCM is pick greatest power for each prime so 2^3 \* 3^1 whereas gcd is take lowest power for each prime so 2^2 \* 3^0.

So first lets use sieve to create factorizations. Now we can observe through given examples that if that prime number appeared in fewer than n-1 means others have it as 0 so multiply num^0 \(or simply ignore it\) otherwise always take second minimum because we have to work on LCM pairs so one minimum will ultimately get lost. if n==1 we take most minimum because we don't have power 0 case in factors.

```cpp
const int MAXN = 2e5 + 5;
// creates prime factorization
// so for 12 vector will contain -> {2, 2} {3, 1} means 2^2 . 3
vector<pii> primes[MAXN];
void sieve()
{
    for (int i = 2; i < MAXN; ++i)
    {
        if (!primes[i].empty()) continue;
        for (int j = i; j < MAXN; j += i)
        {
            int q = j;
            pii nxt = {i, 0};
            while (q%i == 0) q /= i, ++nxt.S;
            primes[j].push_back(nxt);
        }
    }
}
vec<1, int> factors[MAXN];
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    sieve();
    int n; cin >> n;
    vec<1, int> arr(n);
    for (auto &x : arr) cin >> x;
    for (auto &x : arr)
        for (auto &y : primes[x]) { factors[y.F].push_back(y.S); bug(y.F, y.S); }
    int ans = 1;
    for (int i = 2; i < MAXN; ++i)
    {
        sort(all(factors[i]));
        if (sz(factors[i]) == n-1) ans *= pow(i, factors[i][0]);
        else if (sz(factors[i]) == n) ans *= pow(i, factors[i][1]);
    }
    cout << ans << '\n';
    return 0;
}
```

### [1352C - K-th Not Divisible by n](https://codeforces.com/contest/1352/problem/C)

Simple Binary Search

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int n, k; cin >> n >> k;
        int l = 0, r = 1e18;
        while (l+1 < r)
        {
            int mid = (l+r)/2;
            int pos = mid - mid/n;
            if (pos >= k) r = mid;
            else l = mid;
        }
        int a = r - r/n, b = (r+1) - (r+1)/n;
        if (a == k) cout << r << '\n';
        else cout << r+1 << '\n';
    }
    return 0;
}
```

### [1359C - Mixing Water](https://codeforces.com/contest/1359/problem/C)

```cpp
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

### [1359D - Yet Another Yet Another Task](https://codeforces.com/contest/1359/problem/D)

```cpp
#define INF (1LL<<61)
int solve(vec<1, int> &arr, int x)
{
    int res = -INF;
    for (int i = 0; i < sz(arr); ++i)
    {
        if (arr[i] > x) continue;
        int cur = 0;
        while (i < sz(arr))
        {
            if (arr[i] > x) break;
            else cur = max(arr[i], cur+arr[i]);
            res = max(res, cur);
            ++i;
        }
    }
    res -= x;
    return res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, int> arr(n);
    for (auto &x : arr) cin >> x;
    int ans = 0;
    for (int i = -30; i <= 30; ++i)
        ans = max(ans, solve(arr, i));
    cout << ans << '\n';
    return 0;
}
```

### [1369E - Modular Stability](https://codeforces.com/contest/1359/problem/E)

Write brute force see observation, [brute force on n=4 k=8](https://paste.ubuntu.com/p/JKSfbcbC5k/) so observation is to go with gaps initially 1 then we want to choose k-1 elements out of n-1. If gap was 2 then we want to choose k-1 out of n/2 - 1 and so on making it choose k-1 from n/x - 1 where x is from 1 to 5e5

```cpp
const int MAXN= 1e6+5;
int fact[MAXN+1], inv[MAXN+1];
void nCrPreprocess()
{
    fact[0] = inv[0] = 1;
    for(int i = 1; i <= MAXN; ++i)
    {
        fact[i] = multiply(fact[i-1], i);
        inv[i] = powMod(fact[i], MOD-2);
    }
}
int nCrQuery(int n, int r)
{
    if (n < r) return 0;
    int res = 1; res = multiply(res, fact[n]);
    res = multiply(res, inv[r]);
    res = multiply(res, inv[n-r]);
    return res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, k; cin >> n >> k;
    nCrPreprocess();
    if (k == 1) cout << n << '\n';
    else
    {
        int ans = 0;
        for (int x = 1; x <= 5e5; ++x)
        {
            if (x*k > n) break;
            (ans += nCrQuery(n/x - 1, k-1)) %= MOD;
        }
        cout << ans << '\n';
    }
    return 0;
}
```

## Day 5 \(6\)

### [1345B - Card Constructions](https://codeforces.com/contest/1345/problem/B)

Easy but required some long long handling stuff

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        ll n; cin >> n;
        ll ans = 0;
        while (n >= 2)
        {
            int h = (int)(sqrt(1 + 24*n) - 1)/6;
            n -= (h*(3*h + 1))/2, ans++;
        }
        cout << ans << '\n';
    }
    return 0;
}
```

### [1348B - Phoenix and Beauty](https://codeforces.com/contest/1348/problem/B)

Easy logical problem, to make all subarray of length k equal we have to make it periodic on k

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int n, k; cin >> n >> k;
        set<int> st;
        for (int i = 0; i < n; ++i) { int x; cin >> x; st.insert(x); }
        if (sz(st) > k) cout << "-1\n";
        else
        {
            cout << n*k << '\n';
            for (int i = 0; i < n; ++i)
            {
                for (auto &x : st) cout << x << " ";
                int rem = k-sz(st);
                while (rem--) cout << "1 ";
            }
            cout << '\n';
        }
    }
    return 0;
}
```

### [1348D - Phoenix and Science](https://codeforces.com/contest/1348/problem/D)

Nice observation based problem

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int n; cin >> n;
        vec<1, int> arr;
        for (int i = 1; i <= n; i *= 2) { arr.push_back(i); n -= i; }
        if (n) arr.push_back(n);
        sort(all(arr));
        cout << sz(arr)-1 << '\n';
        for (int i = 1; i < sz(arr); ++i) cout << arr[i]-arr[i-1] << " ";
        cout << '\n';
    }
    return 0;
}
```

### [1341C - Nastya and Strange Generator](https://codeforces.com/contest/1341/problem/C)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int n; cin >> n;
        intHashTable pos;
        for (int i = 1; i <= n; ++i)
        {
            int x; cin >> x;
            pos[x] = i;
        }
        set<pii> st;
        multiset<int> curCnts;
        for (int i = 1; i <= n; ++i) { st.insert({i, 1}); curCnts.insert(1); }
        bool notPoss = false;
        for (int i = 1; i <= n; ++i)
        {
            auto it = st.lower_bound({pos[i], 0});
            if (it->F != pos[i] || it->S < *curCnts.rbegin()) { notPoss = true; break; }
            int prevCnt = it->S; st.erase(it); curCnts.erase(curCnts.lower_bound(prevCnt));
            auto nxt = st.lower_bound({pos[i], 0});
            if (nxt != st.end())
            {
                curCnts.erase(curCnts.lower_bound(nxt->S));
                curCnts.insert(nxt->S + prevCnt);
                st.insert({nxt->F, nxt->S + prevCnt});
                st.erase(nxt);
            }
        }
        if (notPoss) cout << "No\n";
        else cout << "Yes\n";
    }
    return 0;
}
```

### [1341D - Nastya and Scoreboard](https://codeforces.com/contest/1341/problem/D)

Nice DP problem, requires top down dp \(for greedy choice\) and backtracking

```cpp
// DP O(10ND)
int n, k;
const int MAXN = 2005;
#define INF (1LL<<30)
string num2Str[] = {"1110111", "0010010", "1011101", "1011011", "0111010", "1101011", "1101111", "1010010", "1111111", "1111011"};
vec<2, int> dp(MAXN, MAXN, -1);
vec<2, int> dig(MAXN, MAXN, -1), cnt(MAXN, MAXN, 0);
vec<2, pii> pre(MAXN, MAXN, make_pair(-1, -1));
bool solve(int cur = 0, int flipCnt = 0)
{
    if (cur == n) return (flipCnt == k);
    if (dp[cur][flipCnt] != -1) return dp[cur][flipCnt];
    for (int curDig = 9; curDig >= 0; --curDig)
    {
        int newCnt = flipCnt + cnt[cur][curDig];
        if (newCnt > k) continue;
        if (solve(cur+1, newCnt))
        {
            dig[cur+1][newCnt] = curDig, pre[cur+1][newCnt] = {cur, flipCnt};
            return dp[cur][flipCnt] = true;
        }
    }
    return dp[cur][flipCnt] = false;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    cin >> n >> k;
    vec<1, string> arr(n);
    for (auto &x : arr) cin >> x;
    for (int i = 0; i < n; ++i)
    {
        for (int curDig = 0; curDig <= 9; ++curDig)
        {
            for (int j = 0; j < 7; ++j)
            {
                if (arr[i][j] == '1' && num2Str[curDig][j] == '0') { cnt[i][curDig] = INF; break; }
                else if (arr[i][j] == '0' && num2Str[curDig][j] == '1') cnt[i][curDig]++;
            }
        }
    }

    if (!solve()) cout << "-1\n";
    else
    {
        string res;
        for (int i = n, j = k; dig[i][j] != -1; tie(i, j) = pre[i][j])
            res = (char)(dig[i][j]+'0') + res;
        cout << res << '\n';
    }
    return 0;
}
```

### [1343D - Constant Palindrome Sum](https://codeforces.com/contest/1343/problem/D)

We can pick x from 2 to 2\*k, idea is to use some scanline logic. Consider sample testcase: 6 1 1 7 3 4 6 Now we have n/2 pairs: 6+6, 1+4, 1+3, 7+6 i.e. 12, 5, 4, 13 we can store them in a set or array of 2\*k length to basically count how many already pair we have corresponding to x. For a pair a\[i\] a\[j\] min of a\[i\]+1 and a\[j\]+1 and max of a\[i\]+k and a\[j\]+k denotes that range which is achievable by the pair via replacing one number out of pair. If we increase count of each such range \(using difference array logic\) we can compute finally for a particular x the value easily by: \(pairsAchivableHereByOnly1Flip - pairsAlreadyX\)\*1 since these pairs requires 1 flip and \(pairs - pairsAchivableByOnly1Flip\)\*2 since these requires 2 flips

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int n, k; cin >> n >> k;
        vec<1, int> arr(n), cnt(2*k + 1);
        for (auto &x : arr) cin >> x;
        for (int i = 0, j = n-1; i < j; ++i, --j) cnt[arr[i]+arr[j]]++;
        vec<1, int> prefSm(2*k + 2);
        for (int i = 0, j = n-1; i < j; ++i, --j)
        {
            int l1 = 1+arr[i], r1 = k+arr[i];
            int l2 = 1+arr[j], r2 = k+arr[n-i-1];
            ++prefSm[min(l1, l2)], --prefSm[max(r1, r2)+1];
        }
        partial_sum(all(prefSm), prefSm.begin());
        int ans = (1LL<<61);
        for (int i = 2; i <= 2*k; ++i)
            ans = min(ans, (prefSm[i]-cnt[i]) + (n/2 - prefSm[i])*2);
        cout << ans << '\n';
    }
    return 0;
}
```

## Day 6



