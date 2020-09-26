# CSES Problem Set - I

{% embed url="https://cses.fi/problemset/" %}

## Introductory Problems

### [Weird Algorithm](https://cses.fi/problemset/task/1068)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    while (n != 1)
    {
        cout << n << ' ';
        if (n&1) n = n*3 + 1;
        else n /= 2;
    }
    cout << "1\n";
    return 0;
}
```

### [Missing Number](https://cses.fi/problemset/task/1083)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    int sm = 0;
    for (int i = 0; i < n-1; ++i) { int x; cin >> x; sm += x; }
    cout << ((n*(n+1))/2 - sm) << '\n';
    return 0;
}
```

### [Repetitions](https://cses.fi/problemset/task/1069)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    string str; cin >> str;
    int ans = 1;
    for (int i = 1, cnt = 1; i < str.size(); ++i)
    {
        if (str[i] == str[i-1]) { cnt++; ans = max(ans, cnt); }
        else cnt = 1;
    }
    cout << ans << '\n';
    return 0;
}
```

### [Increasing Array](https://cses.fi/problemset/task/1094)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, int> arr(n);
    for (auto &x : arr) cin >> x;
    int ans = 0;
    for (int i = 1; i < n; ++i)
        if (arr[i] < arr[i-1]) { ans += (arr[i-1]-arr[i]); arr[i] = arr[i-1]; }
    cout << ans << '\n';
    return 0;
}
```

### [Permutations](https://cses.fi/problemset/task/1070/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    if (n == 1) cout << "1\n";
    else if (n < 4) cout << "NO SOLUTION\n";
    else
    {
        for (int i = 2; i <= n; i += 2) cout << i << " ";
        for (int i = 1; i <= n; i += 2) cout << i << " ";
        cout << '\n';
    }
    return 0;
}
```

### [Number Spiral](https://cses.fi/problemset/task/1071/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int y, x; cin >> y >> x;
        int k = max(x, y);
        int ans = (k-1)*(k-1);
        if (k&1) ans += (x + (k-y));
        else ans += (y + (k-x));
        cout << ans << '\n';
    }
    return 0;
}
```

### [Two Knights](https://cses.fi/problemset/task/1072/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    for (int i = 1; i <= n; ++i)
    {
        int res = ((i-1)*(i+4)*(i*i - 3*i + 4)) / 2;
        cout << res << '\n';
    }
    return 0;
}
```

### [Two Sets](https://cses.fi/problemset/task/1092/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    if ((n * (n+1)) % 4 == 0)
    {
        cout << "YES\n";
        set<int> a, b;
        for (int i = 1; i <= n; ++i) a.insert(i);
        int x = (n*(n+1))/4;
        
        for (int i = n; i >= 1; --i)
            if (i <= x) { x -= i; a.erase(i); b.insert(i); }
        cout << a.size() << '\n';
        for (auto &x : a) cout << x << " "; cout << '\n';
        cout << b.size() << '\n';
        for (auto &x : b) cout << x << " "; cout << '\n';
    }
    else cout << "NO\n";
    return 0;
}
```

### [Bit Strings](https://cses.fi/problemset/task/1617/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    cout << powMod(2, n) << '\n';
    return 0;
}
```

### [Trailing Zeros](https://cses.fi/problemset/task/1618/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    int ans = 0;
    for (int x = 5; x <= n; x *= 5)
        ans += (n/x);
    cout << ans << '\n';
    return 0;
}
```

### [Coin Piles](https://cses.fi/problemset/task/1754)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t; cin >> t;
    while (t--)
    {
        int a, b; cin >> a >> b;
        if ((2*a - b)%3 == 0 && (2*b - a)%3 == 0)
        {
            int x = (2*a - b)/3, y = (2*b - a)/3;
            if (x >= 0 && y >= 0 && x <= min(a, b) && y <= min(a, b)) cout << "YES\n";
            else cout << "NO\n";
        }
        else cout << "NO\n";
    }
    return 0;
}
```

### [Palindrome Reorder](https://cses.fi/problemset/task/1755)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    string str; cin >> str;
    vec<1, int> cnt(26, 0);
    for (auto &x : str) cnt[x-'A']++;
    bool allow = (str.size()%2 == 1);
    string side, m = "";
    for (int i = 0; i < 26; ++i)
    {
        if (cnt[i] == 0) continue;
        if (cnt[i]&1)
        {
            if (allow) { allow = false; m = string(cnt[i], ('A'+i)); }
            else { cout << "NO SOLUTION\n"; return 0; }
        }
        else side += string(cnt[i]/2, ('A'+i));;
    }
    string otherSide = side;
    reverse(otherSide.begin(), otherSide.end());
    cout << (side + m + otherSide) << '\n';
    return 0;
}
```

### [Creating Strings I](https://cses.fi/problemset/task/1622)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    string str; cin >> str;
    sort(str.begin(), str.end());
    vector<string> res;
    do { res.push_back(str); }
    while (next_permutation(str.begin(), str.end()));
    cout << res.size() << '\n';
    for (auto &x : res) cout << x << '\n';
    return 0;
}
```

### [Apple Division](https://cses.fi/problemset/task/1623)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, int> arr(n);
    int sm = 0;
    for (auto &x : arr) { cin >> x; sm += x; }
    int res = INT_MAX;
    for (int i = 1; i < (1<<n); ++i)
    {
        int x = 0, y = 0, cur = i, pos = 0;
        while (cur)
        {
            if (cur&1) x += arr[pos];
            pos++; cur >>= 1;
        }
        y = sm-x;
        res = min(res, abs(y-x));
    }
    cout << res << '\n';
    return 0;
}
```

### [Chessboard and Queens](https://cses.fi/problemset/task/1624)

```cpp
vec<2, bool> board(8, 8);
int solve(int j = 0, int x = 0, int d1 = 0, int d2 = 0)
{
    if (j == 8) return 1;
    int res = 0;
    for (int i = 0; i < 8; ++i)
    {
        if (board[i][j] && !(x&(1<<i)) && !(d1&(1<<(i+j))) && !(d2&(1<<(i-j+8))))
            res += solve(j+1, (x|(1<<i)), (d1|(1<<(i+j))), (d2|(1<<(i-j+8))));
    }
    return res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    for (int i = 0; i < 8; ++i)
    {
        string str; cin >> str;
        for (int j = 0; j < 8; ++j) board[i][j] = (str[j] != '*');
    }
    cout << solve() << '\n';
    return 0;
}
```

### [Grid Paths](https://cses.fi/problemset/task/1625)

Regular backtracking will time limit, requires pruning.

* If we reached last pos before last move then return 0
* If we hit a wall from where left and right both sides are unvisited then no matter which we will visit other will be unvisited so return 0.

```cpp
const int N = 7;
bool visited[N][N];
int dx[] = {0, +1, 0, -1}, dy[] = {-1, 0, +1, 0};
int solve(string &str, int cur = 0, int y = 0, int x = 0)
{
    if (y == N-1 && x == 0) return (cur == (N*N - 1));
    if (((y+1 == N || (visited[y-1][x] && visited[y+1][x])) && x-1 >= 0 && x+1 < N && !visited[y][x-1] && !visited[y][x+1]) ||
        ((x+1 == N || (visited[y][x-1] && visited[y][x+1])) && y-1 >= 0 && y+1 < N && !visited[y-1][x] && !visited[y+1][x]) ||
        ((y == 0 || (visited[y+1][x] && visited[y-1][x])) && x-1 >= 0 && x+1 < N && !visited[y][x-1] && !visited[y][x+1]) ||
        ((x == 0 || (visited[y][x+1] && visited[y][x-1])) && y-1 >= 0 && y+1 < N && !visited[y-1][x] && !visited[y+1][x]))
        return 0;

    visited[y][x] = true;
    int res = 0;
    for (int i = 0; i < 4; ++i)
    {
        if ((str[cur] == 'U' && i != 0) || (str[cur] == 'R' && i != 1) ||
            (str[cur] == 'D' && i != 2) || (str[cur] == 'L' && i != 3)) continue;
        int _y = y+dy[i], _x = x+dx[i];
        if (_y >= 0 && _x >= 0 && _y < N && _x < N && !visited[_y][_x]) res += solve(str, cur+1, _y, _x);
    }
    visited[y][x] = false;
    return res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    string str; cin >> str;
    cout << solve(str) << '\n';
    return 0;
}
```

## Sorting and Searching

### [Distinct Numbers](https://cses.fi/problemset/task/1621/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    set<int> st;
    int x;
    for (int i = 0; i < n; ++i) { cin >> x; st.insert(x); }
    cout << st.size() << '\n';
    return 0;
}
```

### [Apartments](https://cses.fi/problemset/task/1084/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m, k; cin >> n >> m >> k;
    vec<1, int> a(n), b(m);
    for (auto &x : a) cin >> x;
    for (auto &x : b) cin >> x;
    sort(all(a)); sort(all(b));
    int res = 0;
    for (int i = 0, j = 0; i < n && j < m;)
    {
        if (abs(a[i] - b[j]) <= k) i++, j++, res++;
        else if (a[i] > b[j]+k) j++;
        else i++;
    }
    cout << res << '\n';
    return 0;
}
```

### [Ferris Wheel](https://cses.fi/problemset/task/1090/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, x; cin >> n >> x;
    vec<1, int> arr(n);
    for (auto &v : arr) cin >> v;
    sort(all(arr));
    int res = 0;
    for (int i = 0, j = n-1; i <= j; ++i, --j)
    {
        if (i == j) { res++; continue; }
        while (i < j && arr[i]+arr[j] > x) --j, ++res;
        res++;
    }
    cout << res << '\n';
    return 0;
}
```

### [Concert Tickets](https://cses.fi/problemset/task/1091/)

We want atmost x value for that we can add greater&lt;int&gt; comparator in our multiset making our lowerbound give atmost x instead of atleast x.

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    multiset<int, greater<int>> st;
    while (n--) { int x; cin >> x; st.insert(x); }
    while (m--)
    {
        int x; cin >> x;
        auto it = st.lower_bound(x);
        if (it == st.end()) cout << "-1\n";
        else { cout << *it << '\n'; st.erase(it); }
    }
    return 0;
}
```

### [Restaurant Customers](https://cses.fi/problemset/task/1619/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, pii> a(n);
    for (auto &x : a) cin >> x.F >> x.S;
    sort(all(a));
    priority_queue<int, vector<int>, greater<int>> pq;
    int res = 0;
    for (auto &x : a)
    {
        while (!pq.empty() && pq.top() < x.F) pq.pop();
        pq.push(x.S);
        res = max(res, (int)pq.size());
    }
    cout << res << '\n';
    return 0;
}
```

### [Movie Festival](https://cses.fi/problemset/task/1629/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    vec<1, pii> a(n);
    for (auto &x : a) cin >> x.S >> x.F;
    sort(all(a));
    int prev = -1, res = 0;
    for (auto &x : a)
    {
        if (x.S < prev) continue;
        res++;
        prev = x.F;
    }
    cout << res << '\n';
    return 0;
}
```

### [Sum of Two Values](https://cses.fi/problemset/task/1640/)

```cpp
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, x; cin >> n >> x;
    vector<pii> a(n);
    for (int i = 0; i < n; ++i) { int v; cin >> v; a[i] = {v, i}; }
    sort(all(a));
    for (int i = 0, j = n-1; i < j;)
    {
        if (a[i].F + a[j].F == x) { cout << a[i].S+1 << " " << a[j].S+1 << '\n'; return 0; }
        if (a[i].F + a[j].F < x) i++;
        else j--;
    }
    cout << "IMPOSSIBLE\n";
    return 0;
}
```

### [Maximum Subarray Sum](https://cses.fi/problemset/task/1643/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;
		vec<1, int> dp(n);
		dp[0] = arr[0];
		for (int i = 1; i < n; ++i) dp[i] = max(arr[i], dp[i-1]+arr[i]);
		cout << *max_element(all(dp)) << '\n';
		return 0;
}
```

### [Stick Lengths](https://cses.fi/problemset/task/1074/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, int> a(n);
		for (auto &x : a) cin >> x;
		sort(all(a));
		int m = a[n/2], res = 0;
		for (int i = 0; i < n; i++) res += abs(a[i]-m);
		cout << res << endl;
		return 0;
}
```

### [Playlist](https://cses.fi/problemset/task/1141/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;
		map<int, int> cnt;
		int res = 0;
		for (int i = 0, j = 0; j < n; ++j)
		{
				while (cnt[arr[j]] > 0) cnt[arr[i++]]--;
				cnt[arr[j]]++;
				res = max(res, j-i+1);
		}
		cout << res << '\n';
		return 0;
}
```

### [Towers](https://cses.fi/problemset/task/1073/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		multiset<int> st;
		for (int i = 0; i < n; ++i)
		{
				int x; cin >> x;
				auto it = st.upper_bound(x);
				if (it != st.end()) st.erase(it);
				st.insert(x);
		}
		cout << st.size() << '\n';
		return 0;
}
```

### [Traffic Lights](https://cses.fi/problemset/task/1163/)

Maintain a timeline initially 0------x then say a cut 0-----z z-----x  
We can represent it like this \(x, x\) -&gt; \(z, z\) \(x-z, x\)  
Here first means length of the segment and second means end point, if processed under a set rbegin first will store the desired result and processing can be done in logN. However to do processing we need to find segment with second atleast v \(query num\) for that we have to maintain a reverse set with first and second swapped.

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int x, n; cin >> x >> n;
		set<pii> a, b;
		a.insert({x, x}); b.insert({x, x});
		while (n--)
		{
				int v; cin >> v;
				auto it = a.lower_bound({v, 0});
				auto cur = *it;

				a.erase(it);
				a.insert({v, cur.S - (cur.F-v)});
				a.insert({cur.F, cur.F - v});

				b.erase({cur.S, cur.F});
				b.insert({cur.S - (cur.F-v), v});
				b.insert({cur.F - v, cur.F});

				cout << b.rbegin()->F << ' ';
		}
		cout << '\n';
		return 0;
}
```

### [Room Allocation](https://cses.fi/problemset/task/1164/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, pair<pii, int>> arr(n);
		for (int i = 0; i < n; ++i) { cin >> arr[i].F.F >> arr[i].F.S; arr[i].S = i; }
		sort(all(arr));

		set<int> rooms;
		for (int i = 1; i <= n; ++i) rooms.insert(i);
		priority_queue<pii, vector<pii>, greater<pii>> pq;
		vec<1, int> res(n);
		int mx = 0;
		for (auto &x : arr)
		{
				while (!pq.empty() && pq.top().F < x.F.F) { rooms.insert(pq.top().S); pq.pop(); }
				int cur = *rooms.begin(); rooms.erase(cur);
				res[x.S] = cur;
				pq.push({x.F.S, cur});
				mx = max(mx, cur);
		}
		cout << mx << '\n';
		for (auto &x : res) cout << x << " "; cout << '\n';
		return 0;
}
```

### [Factory Machines](https://cses.fi/problemset/task/1620/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, t; cin >> n >> t;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;
		int l = -1, r = 1e18;
		auto check = [&](int mid)
		{
				int cur = 0;
				for (auto &x : arr)
				{
						cur += (mid/x);
						if (cur >= t) return true;
				}
				return false;
		};
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

### [Tasks and Deadlines](https://cses.fi/problemset/task/1630/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, pii> arr(n);
		for (auto &x : arr) cin >> x.F >> x.S;
		sort(all(arr));
		int cur = 0, res = 0;
		for (auto &x : arr)
		{
				cur += x.F;
				res += (x.S - cur);
		}
		cout << res << '\n';
		return 0;
}
```

### [Reading Books](https://cses.fi/problemset/task/1631/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		int mx = 0, sm = 0;
		for (int i = 0; i < n; ++i)
		{
				int x; cin >> x;
				mx = max(mx, x);
				sm += x;
		}
		cout << (mx > (sm-mx) ? 2*mx : sm) << '\n';
		return 0;
}
```

### [Sum of Three Values](https://cses.fi/problemset/task/1641/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, x; cin >> n >> x;
		vec<1, pii> arr(n);
		for (int i = 0; i < n; ++i) { cin >> arr[i].F; arr[i].S = i+1; }
		sort(all(arr));
		for (int i = 0; i < n; ++i)
		{
				int reqd = x - arr[i].F;
				for (int j = i+1, k = n-1; j < k; )
				{
						if (arr[j].F+arr[k].F == reqd) { cout << arr[i].S << " " << arr[j].S << " " << arr[k].S << '\n'; return 0; }
						else if (arr[j].F+arr[k].F < reqd) ++j;
						else --k;
				}
		}
		cout << "IMPOSSIBLE\n";
		return 0;
}
```

### [Sum of Four Values](https://cses.fi/problemset/task/1642/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, x; cin >> n >> x;
		vec<1, pii> arr(n);
		for (int i = 0; i < n; ++i) { cin >> arr[i].F; arr[i].S = i+1; }
		sort(all(arr));
		for (int p = 0; p < n; ++p)
		{
				for (int q = p+1; q < n; ++q)
				{
						int reqd = x - arr[p].F - arr[q].F;
						for (int r = q+1, s = n-1; r < s;)
						{
								if (arr[r].F+arr[s].F == reqd) { cout << arr[p].S << " " << arr[q].S << " " << arr[r].S << " " << arr[s].S << '\n'; return 0; }
								else if (arr[r].F+arr[s].F < reqd) ++r;
								else --s;
						}
				}
		}
		cout << "IMPOSSIBLE\n";
		return 0;
}
```

### [Nearest Smaller Values](https://cses.fi/problemset/task/1645/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		deque<pii> dq;
		dq.push_back({INT_MIN, 0});
		for (int i = 0; i < n; ++i)
		{
				int x; cin >> x;
				while (dq.back().F >= x) dq.pop_back();
				cout << dq.back().S << ' ';
				dq.push_back({x, i+1});
		}
		cout << '\n';
		return 0;
}
```

### [Subarray Sums I](https://cses.fi/problemset/task/1660/) \(and [Subarray Sums II](https://cses.fi/problemset/task/1661/)\)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, x; cin >> n >> x;
		map<int, int> cnt;
		cnt[0]++;
		int sm = 0, res = 0;
		while (n--)
		{
				int v; cin >> v;
				sm += v;
				res += cnt[sm-x];
				cnt[sm]++;
		}
		cout << res << '\n';
		return 0;
}
```

### [Subarray Divisibility](https://cses.fi/problemset/task/1662/)

If we keep prefix sum, X denotes prefix sum at some point x and Y denotes prefix sum at some other point y. y is always &lt; x. Now we want \(X-Y\) % n = 0 for the condition to hold. opening it we have \(X%n - Y%n + n\) % n = 0.

So for a point we are simply looking for previously looking reqd var and adding its count.

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, int> arr(n+1); arr[0] = 0;
		map<int, int> cnt; cnt[0]++;
		int res = 0;
		for (int i = 1; i <= n; ++i)
		{
				cin >> arr[i];
				(arr[i] += arr[i-1]) %= n;
				if (arr[i] < 0) arr[i] += n;

				int reqd = (arr[i] + n) % n;
				res += cnt[reqd];

				cnt[arr[i]]++;
		}
		cout << res << '\n';
		return 0;
}
```

### [Array Division](https://cses.fi/problemset/task/1085/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, k; cin >> n >> k;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;
 
		auto check = [&](int x)
		{
				int cur = 0, cnt = 0;
				for (auto &v : arr)
				{
						if (v > x) return false;
						if (cur+v > x) cnt++, cur = v;
						else cur += v;
				}
				if (cur) cnt++;
				return (cnt <= k);
		};

		int l = 0, r = accumulate(all(arr), 0LL);
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

### [Sliding Median](https://cses.fi/problemset/task/1076/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, k; cin >> n >> k;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;

		orderedMultiset st;
		auto median = [&]() { return (k&1) ? *st.find_by_order(k/2) : (*st.find_by_order(k/2 - 1) + *st.find_by_order(k/2 - 1))/2; };
		for (int i = 0; i < k; ++i) st.insert(arr[i]);
		cout << median() << ' ';
		for (int i = k; i < n; ++i)
		{
				st.erase(st.upper_bound(arr[i-k]));
				st.insert(arr[i]);
				cout << median() << ' ';
		}
		cout << '\n';
		return 0;
}
```

### [Sliding Cost](https://cses.fi/problemset/task/1077/)

Extending the code of previous problem, we can iterate over k after finding median to find cost for first window now we have to find subsequent costs by some mathematical logic in constant time.

First thing we do is add abs\(m - arr\[i\]\) because that's the cost of new element to the window and remove abs\(oldM - arr\[i-k\]\) because that's the cost of old element. Now what about middle common one's?

![](../.gitbook/assets/image%20%2850%29.png)

Consider above \*artistic\* drawing. Median is nothing but distance first \(black\) was our median so red lines are distance sum of them is our cost. When we shift the median, individually their distances change but they will cancel out except one \(if it's odd element common otherwise there won't be any affect\). So we are checking k%2 \(k means k-1 common so if k is even we have common elements and we need to manually adjust it.

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, k; cin >> n >> k;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;

		orderedMultiset st;
		auto median = [&]() { return (k&1) ? *st.find_by_order(k/2) : (*st.find_by_order(k/2 - 1) + *st.find_by_order(k/2 - 1))/2; };
		for (int i = 0; i < k; ++i) st.insert(arr[i]);

		int oldM = median(), d = 0;
		for (int i = 0; i < k; ++i) d += abs(arr[i]-oldM);
		cout << d << ' ';
		for (int i = k; i < n; ++i)
		{
				st.erase(st.upper_bound(arr[i-k]));
				st.insert(arr[i]);
				int m = median();
				d += abs(m - arr[i]) - abs(oldM - arr[i-k]);
				if (k%2 == 0) d -= (m - oldM);
				oldM = m;
				cout << d << ' ';
		}
		cout << '\n';
		return 0;
}
```

### [Movie Festival II](https://cses.fi/problemset/task/1632/)

Extending traditional variant of this problem, multiset denotes persons last watch time initially zero we iterate over all movie and see if there's any person available with atmost current movie start time then take it. For atmost we just use greater comparator initially lower\_bound means atleast due to this comparator its atmost now.

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, k; cin >> n >> k;
		vec<1, pii> arr(n);
		for (auto &x : arr) cin >> x.S >> x.F;
		sort(all(arr));
		multiset<int, greater<int>> cur;
		while (k--) cur.insert(0);
		int res = 0;
		for (auto &x : arr)
		{
				auto it = cur.lower_bound(x.S);
				if (it != cur.end())
				{
						cur.erase(it);
						cur.insert(x.F);
						res++;
				}
		}
		cout << res << '\n';
		return 0;
}
```

### [Maximum Subarray Sum II](https://cses.fi/problemset/task/1644/)

If we keep prefix sum \(1 based keeping arr\[0\] = 0\) then at some point j we want to find a point i such that i is atleast a index away from b for that we will maintain a sliding window minimum of length k = b-a+1.

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, a, b; cin >> n >> a >> b;
		vec<1, int> arr(n+1);
		arr[0] = 0;
		for (int i = 1; i <= n; ++i) { cin >> arr[i]; arr[i] += arr[i-1]; }

		deque<int> dq;
		int k = b-a+1;
		int res = LONG_LONG_MIN;
		for (int i = 0, j = a; j <= n; ++i, ++j)
		{
				while (!dq.empty() && dq.front() < i-k+1) dq.pop_front();
				while (!dq.empty() && arr[i] < arr[dq.back()]) dq.pop_back();
				dq.push_back(i);
				res = max(res, arr[j]-arr[dq.front()]);
		}
		cout << res << '\n';
		return 0;
}
```

## Dynamic Programming

### [Dice Combinations](https://cses.fi/problemset/task/1633/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, int> dp(n+1, 0);
		dp[0] = 1;
		for (int i = 0; i <= n; ++i)
		{
				if (dp[i] == 0) continue;
				for (int j = i+1; j <= min(i+6, n); ++j) (dp[j] += dp[i]) %= MOD;
		}
		cout << dp[n] << '\n';
		return 0;
}
```

### [Minimizing Coins](https://cses.fi/problemset/task/1634/)

```cpp
#define INF (1LL<<30)
vec<2, int> dp(105, 1000005, INF);
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, x; cin >> n >> x;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;

		dp[0][0] = 0;
		for (int i = 1; i <= n; ++i)
		for (int j = 0; j <= x; ++j)
				dp[i][j] = (j < arr[i-1]) ? dp[i-1][j] : min(dp[i-1][j], 1 + dp[i][j-arr[i-1]]);
		if (dp[n][x] == INF) cout << "-1\n";
		else cout << dp[n][x] << '\n';
		return 0;
}
```

### [Coin Combinations I](https://cses.fi/problemset/task/1635/)

```cpp
vec<1, int> dp(1000005, 0);
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, x; cin >> n >> x;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;

		dp[0] = 1;
		for (int i = 0; i < x; ++i)
				for (int j : arr)
						if (i+j <= x) (dp[i+j] += dp[i]) %= MOD;
		cout << dp[x] << '\n';
		return 0;
}
```

### [Coin Combinations II](https://cses.fi/problemset/task/1636/)

```cpp
/* We want to make this type of dp now (eg: [2 3 5] 9)
          0 1 2 3 4 5 6 7 8 9
2    -    1 0 1 0 1 0 1 0 1 0 
3    -    1 0 1 1 1 1 2 1 2 2 
5    -    1 0 1 1 1 2 2 2 3 3 */
vec<1, int> dp(1000005, 0);
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, x; cin >> n >> x;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;

		dp[0] = 1;
		for (int j : arr)
				for (int i = 0; i < x; ++i)
						if (i+j <= x) (dp[i+j] += dp[i]) %= MOD;
		cout << dp[x] << '\n';
		return 0;
}
```

### [Removing Digits](https://cses.fi/problemset/task/1637/)

```cpp
#define INF (1<<30)
vec<1, int> dp(1000005);
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		for (int i = 0; i <= 9; ++i) dp[i] = 1;
		for (int i = 10; i <= n; ++i)
		{
				int x = i;
				dp[i] = INF;
				while (x)
				{
						dp[i] = min(dp[i], 1 + dp[i - (x%10)]);
						x /= 10;
				}
		}
		cout << dp[n] << '\n';
		return 0;
}
```

### [Grid Paths](https://cses.fi/problemset/task/1638/)

```cpp
vec<2, int> dp(1005, 1005);
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		for (int i = 0; i < n; ++i)
		{
				string str; cin >> str;
				for (int j = 0; j < n; ++j) dp[i][j] = (str[j] == '*') ? -1 : 0;
		}
		dp[0][0] = (dp[0][0] != -1);
		for (int i = 0; i < n; ++i)
		{
				for (int j = 0; j < n; ++j)
				{
						if (i == 0 && j == 0) continue;
						if (dp[i][j] == -1) { dp[i][j] = 0; continue; }
						if (i-1 >= 0) (dp[i][j] += dp[i-1][j]) %= MOD;
						if (j-1 >= 0) (dp[i][j] += dp[i][j-1]) %= MOD;
				}
		}
		cout << dp[n-1][n-1] << '\n';
		return 0;
}
```

### [Book Shop](https://cses.fi/problemset/task/1158/)

```cpp
vec<2, int> dp(1005, 100005, 0);
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, x; cin >> n >> x;
		vec<1, int> h(n), s(n);
		for (auto &x : h) cin >> x;
		for (auto &x : s) cin >> x;

		for (int j = h[0]; j <= x; ++j) dp[0][j] = s[0];
		for (int i = 1; i < n; ++i)
				for (int j = 0; j <= x; ++j)
						dp[i][j] = (j < h[i]) ? dp[i-1][j] : max(dp[i-1][j], s[i] + dp[i-1][j-h[i]]);
		cout << dp[n-1][x] << '\n';
		return 0;
}
```

### [Array Description](https://cses.fi/problemset/task/1746/)

```cpp
vec<2, int> dp(105, 100005, -1);
int solve(int &m, vec<1, int> &arr, int prev = -1, int cur = 0)
{
		if (cur == arr.size()) return 1;
		if (cur != 0 && arr[cur] != 0 && abs(arr[cur]-prev) > 1) return 0;
		if (dp[prev+1][cur] != -1) return dp[prev+1][cur];

		int res = 0;
		if (arr[cur] == 0)
		{
				if (cur == 0)
				{
						for (int i = 1; i <= m; ++i)
								(res += solve(m, arr, i, cur+1)) %= MOD;
				}
				else
				{
						for (int i : {prev-1, prev, prev+1})
								if (i >= 1 && i <= m) (res += solve(m, arr, i, cur+1)) %= MOD;
				}
		}
		else (res += solve(m, arr, arr[cur], cur+1)) %= MOD;
		return dp[prev+1][cur] = res;
}
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;
		cout << solve(m, arr) << '\n';
		return 0;
}
```

### [Edit Distance](https://cses.fi/problemset/task/1639/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		string x, y; cin >> x >> y;
		int n = x.size(), m = y.size();
		vec<2, int> dp(n+1, m+1);
		dp[0][0] = 0;
		for (int j = 1; j <= m; ++j) dp[0][j] = j;
		for (int i = 1; i <= n; ++i) dp[i][0] = i;
		for (int i = 1; i <= n; ++i)
				for (int j = 1; j <= m; ++j)
						dp[i][j] = (x[i-1] == y[j-1]) ? dp[i-1][j-1] : min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]})+1;
		cout << dp[n][m] << '\n';
		return 0;
}
```

### [Rectangle Cutting](https://cses.fi/problemset/task/1744/)

```cpp
#define INF (1<<30)
vec<2, int> dp(505, 505, -1);
int solve(int x, int y)
{
		if (x == y) return 0;
		if (x == 2*y || y == 2*x) return 1;
		if (dp[x][y] != -1) return dp[x][y];
		int res = INF;
		for (int i = 1; i <= x/2; ++i)
				res = min(res, 1 + solve(i, y) + solve(x-i, y));
		for (int i = 1; i <= y/2; ++i)
				res = min(res, 1 + solve(x, i) + solve(x, y-i));
		return dp[x][y] = res;
}

signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int x, y; cin >> x >> y;
		cout << solve(x, y) << '\n';
		return 0;
}
```

### [Money Sums](https://cses.fi/problemset/task/1745/)

```cpp
vec<2, bool> dp(105, 100005, false);
set<int> st;
void solve(vec<1, int> &arr, int cur = 0, int sm = 0)
{
		if (dp[cur][sm]) return;
		if (cur == arr.size()) { st.insert(sm); return; }

		solve(arr, cur+1, sm + arr[cur]);
		solve(arr, cur+1, sm);
		dp[cur][sm] = true;
}
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;
		solve(arr);
		st.erase(0);
		cout << st.size() << '\n';
		for (auto &x : st) cout << x << " ";
		return 0;
}
```

### [Removal Game](https://cses.fi/problemset/task/1097/)

```cpp
vec<2, pii> dp(5005, 5005, make_pair(0, 0));
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		for (int i = 0; i < n; ++i) cin >> dp[i][i].F;
		for (int i = 1; i < n; ++i)
		{
				for (int j = 0, k = i; k < n; ++j, ++k)
				{
						pii x = {dp[j][j].F + dp[j+1][k].S, dp[j][j].S + dp[j+1][k].F};
						pii y = {dp[k][k].F + dp[j][k-1].S, dp[k][k].S + dp[j][k-1].F};
						dp[j][k] = (x.F > y.F) ? x : y;
				}
		}
		cout << dp[0][n-1].F << '\n';
		return 0;
}
```

### [Two Sets II](https://cses.fi/problemset/task/1093/)

```cpp
vec<2, int> dp(505, 250005, -1);
int solve(int &n, int &m, int cur = 1, int sm = 0)
{
		if (cur == n) return (sm == m);
		if (dp[cur][sm] != -1) return dp[cur][sm];

		int res = 0;
		(res += solve(n, m, cur+1, sm)) %= MOD;
		(res += solve(n, m, cur+1, sm+cur)) %= MOD;
		return dp[cur][sm] = res;
}
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		int m = (n*(n+1))/2;
		if (m&1) cout << "0\n";
		else
		{
				m /= 2;
				cout << solve(n, m) << '\n';
		}
		return 0;
}
```

### [Increasing Subsequence](https://cses.fi/problemset/task/1145/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n; cin >> n;
		vec<1, int> arr(n);
		for (auto &x : arr) cin >> x;
		set<int> st;
		for (int i = 0; i < arr.size(); ++i)
		{
				if (st.lower_bound(arr[i]) != st.end())
						st.erase(st.lower_bound(arr[i]));
				st.insert(arr[i]);
		}
		cout << st.size() << '\n';
		return 0;
}
```

### [Projects](https://cses.fi/problemset/task/1140/)

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

## Graph Algorithms

### [Counting Rooms](https://cses.fi/problemset/task/1192/)

```cpp
int n, m;
void DFS(vec<2, bool> &mp, vec<2, bool> &visited, int i, int j)
{
		if (i < 0 || i >= n || j < 0 || j >= m || visited[i][j] || !mp[i][j]) return;
		visited[i][j] = true;
		DFS(mp, visited, i+1, j);
		DFS(mp, visited, i-1, j);
		DFS(mp, visited, i, j+1);
		DFS(mp, visited, i, j-1);
}
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		cin >> n >> m;
		vec<2, bool> mp(n, m, false), visited(n, m, false);
		for (int i = 0; i < n; ++i)
		{
				string str; cin >> str;
				for (int j = 0; j < m; ++j) mp[i][j] = (str[j] == '.');
		}
		int res = 0;
		for (int i = 0; i < n; ++i)
		{
				for (int j = 0; j < m; ++j)
				{
						if (visited[i][j] || !mp[i][j]) continue;
						DFS(mp, visited, i, j);
						res++;
				}
		}
		cout << res << '\n';
		return 0;
}
```

### [Labyrinth](https://cses.fi/problemset/task/1193/)

```cpp
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		vec<2, bool> mp(n, m, false), visited(n, m, false);
		pii start, end;
		for (int i = 0; i < n; ++i)
		{
				string str; cin >> str;
				for (int j = 0; j < m; ++j)
				{
						mp[i][j] = (str[j] != '#');
						if (str[j] == 'A') start = {i, j};
						if (str[j] == 'B') end = {i, j};
				}
		}

		auto check = [&](int i, int j) { return (i >= 0 && i < n && j >= 0 && j < m && mp[i][j] && !visited[i][j]); };

		queue<pii> q;
		q.push(start);
		vec<2, pii> prev(n, m);
		prev[start.F][start.S] = {-1, -1}; visited[start.F][start.S] = true;
		bool possible = false;
		while (!q.empty())
		{
				auto u = q.front();
				q.pop();

				if (u == end) { possible = true; break; }
				if (check(u.F+1, u.S)) { q.push({u.F+1, u.S}); prev[u.F+1][u.S] = u; visited[u.F+1][u.S] = true; }
				if (check(u.F-1, u.S)) { q.push({u.F-1, u.S}); prev[u.F-1][u.S] = u; visited[u.F-1][u.S] = true; }
				if (check(u.F, u.S+1)) { q.push({u.F, u.S+1}); prev[u.F][u.S+1] = u; visited[u.F][u.S+1] = true; }
				if (check(u.F, u.S-1)) { q.push({u.F, u.S-1}); prev[u.F][u.S-1] = u; visited[u.F][u.S-1] = true; }
		}

		if (!possible) cout << "NO\n";
		else
		{
				cout << "YES\n";
				pii lst = {-1, -1};
				stringstream res;
				for (int i = end.F, j = end.S; i != -1 || j != -1; tie(i, j) = prev[i][j])
				{
						if (lst == make_pair(-1, -1)) { lst = {i, j}; continue; }
						if (i-1 == lst.F && j == lst.S) res << 'U';
						else if (i+1 == lst.F && j == lst.S) res << 'D';
						else if (i == lst.F && j+1 == lst.S) res << 'R';
						else if (i == lst.F && j-1 == lst.S) res << 'L';
						lst = {i, j};
				}
				string ans = res.str(); reverse(all(ans));
				cout << ans.size() << '\n' << ans << '\n';
		}
		return 0;
}
```

### [Building Roads](https://cses.fi/problemset/task/1666/)

```cpp
const int MAXN = 1e5+5;
vec<1, int> adj[MAXN];
vec<1, bool> visited(MAXN, false);
void DFS(int u)
{
		visited[u] = true;
		for (auto &v : adj[u])
				if (!visited[v]) DFS(v);
}
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		while (m--)
		{
				int u, v; cin >> u >> v;
				adj[u].push_back(v);
				adj[v].push_back(u);
		}

		vec<1, int> components;
		for (int i = 1; i <= n; ++i)
		{
				if (visited[i]) continue;
				components.push_back(i);
				DFS(i);
		}
		cout << components.size()-1 << '\n';
		for (int i = 1; i < components.size(); ++i)
				cout << components[i-1] << " " << components[i] << '\n';
		return 0;
}
```

### [Message Route](https://cses.fi/problemset/task/1667/)

Can use 0-1 BFS \(O\(E\)\) instead of Dijkstra which works in \(O\(V+E\)\)

```cpp
#define INF (1<<30)
void zeroOneBFS(int s, int n, vector<pii> adj[], vector<int> &dist, vector<int> &prev)
{
		dist.assign(n, INF);
		prev.assign(n, -1);
		dist[s] = 0;
		deque<int> dq;
		dq.push_front(s);
		while (!dq.empty())
		{
				int u = dq.front();
				dq.pop_front();
				for (auto &edge : adj[u])
				{
						int v = edge.first, w = edge.second;
						if (dist[u] + w < dist[v])
						{
								dist[v] = dist[u] + w;
								prev[v] = u;
								if (w == 1) dq.push_back(v);
								else dq.push_front(v);
						}
				}
		}
}
bool findPath(int from, int to, vector<int> &prev, vector<int> &path)
{
		for (int cur = to; cur != -1; cur = prev[cur])
				path.push_back(cur);
		reverse(path.begin(), path.end());
		return (!path.empty() && *path.begin() == from);
}

signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		vec<1, pii> adj[n+1];
		while (m--)
		{
				int u, v; cin >> u >> v;
				adj[u].push_back({v, 1});
				adj[v].push_back({u, 1});
		}
		vec<1, int> dist, prev, path;
		zeroOneBFS(1, n+1, adj, dist, prev);
		if (!findPath(1, n, prev, path)) cout << "IMPOSSIBLE\n";
		else
		{
				cout << path.size() << '\n';
				for (auto &x : path) cout << x << " ";
		}
		return 0;
}
```

### [Building Teams](https://cses.fi/problemset/task/1668/)

```cpp
const int MAXN = 1e5 + 5;
vec<1, int> adj[MAXN], res(MAXN, -1);
vec<1, bool> vis(MAXN, false);
bool notPossible = false;
void DFS(int u, int team = 0)
{
		vis[u] = true; res[u] = team;
		for (auto &v : adj[u])
		{
				if (!vis[v]) DFS(v, !team);
				else if (res[v] == res[u]) { notPossible = true; return; }
				if (notPossible) return;
		}
}

signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		while (m--)
		{
				int u, v; cin >> u >> v;
				adj[u].push_back(v);
				adj[v].push_back(u);
		}
		for (int i = 1; i <= n; ++i)
				if (!vis[i]) DFS(i);
		if (notPossible) cout << "IMPOSSIBLE\n";
		else
		{
				for (int i = 1; i <= n; ++i) cout << res[i]+1 << " ";
				cout << '\n';
		}
		return 0;
}
```

### [Round Trip](https://cses.fi/problemset/task/1669/)

```cpp
const int MAXN = 1e5 + 5;
vec<1, int> adj[MAXN], vis(MAXN, -1), res;
bool found = false, backtracking = false; int backNode = -1;

void DFS(int u, int x = 0, int prev = -1)
{
		vis[u] = x;
		for (auto &v : adj[u])
		{
				if (v == prev) continue;
				if (vis[v] == -1) DFS(v, x+1, u);
				else
				{
						int cur = (x+1) - vis[v];
						res.push_back(v); res.push_back(u);
						if (cur >= 2)
						{
								found = true, backtracking = true; backNode = v;
								return;
						}
				}
				if (backtracking)
				{
						res.push_back(u);
						if (u == backNode) backtracking = false;
				}
				if (found) return;
		}
}

signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		while (m--)
		{
				int u, v; cin >> u >> v;
				adj[u].push_back(v);
				adj[v].push_back(u);
		}
		for (int i = 1; i <= n; ++i)
		{
				if (vis[i] == -1) DFS(i);
				if (found) break;
		}
		if (!found) cout << "IMPOSSIBLE\n";
		else
		{
				cout << res.size() << '\n';
				for (auto &x : res) cout << x << " ";
				cout << '\n';
		}
		return 0;
}
```

### [Monsters](https://cses.fi/problemset/task/1194/)

```cpp
const int dr[] = {0, 1, 0, -1}, dc[] = {1, 0, -1, 0};
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		vec<2, bool> isSafe(n, m), vis(n, m, false);
		vec<1, pii> monsters; pii character;
		for (int i = 0; i < n; ++i)
		{
				string str; cin >> str;
				for (int j = 0; j < m; ++j)
				{
						isSafe[i][j] = (str[j] != '#');
						if (str[j] == 'M') { monsters.push_back({i, j}); isSafe[i][j] = false; }
						else if (str[j] == 'A') character = {i, j};
				}
		}

		queue<pair<pii, bool>> q;
		q.push({character, true});
		for (auto &x : monsters) q.push({x, false});

		bool found = false; pii foundPos;
		vec<2, pii> lst(n, m, make_pair(-1, -1));
		while (!q.empty())
		{
				int sz = q.size();
				while (sz--)
				{
						auto u = q.front(); q.pop();
						if (u.S)
						{
								if (!isSafe[u.F.F][u.F.S]) continue;
								vis[u.F.F][u.F.S] = true;
								if (u.F.F == 0 || u.F.F == n-1 || u.F.S == 0 || u.F.S == m-1) { found = true; foundPos = {u.F.F, u.F.S}; break; }
						}
						else isSafe[u.F.F][u.F.S] = false;
						
						for (int i = 0; i < 4; ++i)
						{
								int newR = u.F.F + dr[i], newC = u.F.S + dc[i];
								if (newR >= 0 && newR < n && newC >= 0 && newC < m && isSafe[newR][newC])
								{
										if (!u.S || (u.S && !vis[newR][newC]))
										{
												q.push({{newR, newC}, u.S});
												if (u.S) lst[newR][newC] = {u.F.F, u.F.S};
										}
								}
						}
				}
				if (found) break;
		}

		if (!found) cout << "NO\n";
		else
		{
				cout << "YES\n";
				pii prev = {-1, -1};
				string res;
				for (int i = foundPos.F, j = foundPos.S; i != -1 || j != -1; tie(i, j) = lst[i][j])
				{
						if (prev.F == -1 && prev.S == -1) { prev = {i, j}; continue; }
						if (i == prev.F && j+1 == prev.S) res += 'R';
						else if (i == prev.F && j-1 == prev.S) res += 'L';
						else if (i+1 == prev.F && j == prev.S) res += 'D';
						else if (i-1 == prev.F && j == prev.S) res += 'U';
						prev = {i, j};
				}
				reverse(res.begin(), res.end());
				cout << res.size() << '\n' << res << '\n';
		}
		return 0;
}
```

### [Shortest Routes I](https://cses.fi/problemset/task/1671/)

```cpp
#define INF (1LL<<61)
void dijkstra(int s, int n, vector<pii> adj[], vector<int> &dist, vector<int> &prev)
{
		dist.assign(n, INF);
		prev.assign(n, -1);
		dist[s] = 0;
		priority_queue<pii, vector<pii>, greater<pii>> pq;
		pq.push({0, s});
		while (!pq.empty())
		{
				int u = pq.top().second, d = pq.top().first;
				pq.pop();
				if (d != dist[u]) continue;
				for (auto &edge : adj[u])
				{
						int v = edge.first, w = edge.second;
						if (dist[u] + w < dist[v])
						{
								dist[v] = dist[u] + w;
								prev[v] = u;
								pq.push({dist[v], v});
						}
				}
		}
}

signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		vec<1, pii> adj[n+1];
		while (m--)
		{
				int u, v, w; cin >> u >> v >> w;
				adj[u].push_back({v, w});
		}
		vec<1, int> dist, prev;
		dijkstra(1, n+1, adj, dist, prev);
		for (int i = 1; i <= n; ++i) cout << dist[i] << " ";
		cout << '\n';
		return 0;
}
```

### [Shortest Routes II](https://cses.fi/problemset/task/1672/)

```cpp
#define INF (1LL<<61)
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m, q; cin >> n >> m >> q;
		vec<2, int> adj(n+1, n+1, INF);
		for (int i = 1; i <= n; ++i) adj[i][i] = 0;
		while (m--)
		{
				int u, v, w; cin >> u >> v >> w;
				adj[u][v] = min(w, adj[u][v]);
				adj[v][u] = min(w, adj[v][u]);
		}
		for (int k = 1; k <= n; ++k)
				for (int i = 1; i <= n; ++i)
						for (int j = 1; j <= n; ++j)
								if (adj[i][k] < INF && adj[k][j] < INF) adj[i][j] = min(adj[i][j], adj[i][k] + adj[k][j]);
		while (q--)
		{
				int u, v; cin >> u >> v;
				if (adj[u][v] == INF) cout << "-1\n";
				else cout << adj[u][v] << '\n';
		}
		return 0;
}
```

### [High Score](https://cses.fi/problemset/task/1673/)

We can use negative edge path finding algorithms like bellman ford, I used SPFA which is more optimized slight modification in it can solve the problem. However the question expects to solve for infinite loops as well \(a cycle with positive sum\) those will result answer to be infinity if the cycle is present within 1 to n so print -1 otherwise if no cycle there simply return result of algorithm.

```cpp
#define INF (1LL<<61)
set<int> st;
bool spfa(int s, int n, vector<pii> adj[], vector<int> &dist, vector<int> &prev)
{
		dist.assign(n, -INF);
		prev.assign(n, -1);
		vector<int> cnt(n, 0);
		vector<bool> inqueue(n, false);
		queue<int> q;
		dist[s] = 0;
		q.push(s);
		inqueue[s] = true;
		bool cycle = false;
		while (!q.empty())
		{
				int u = q.front();
				q.pop();
				inqueue[u] = false;
				for (auto &edge : adj[u])
				{
						int v = edge.first, w = edge.second;
						if (dist[u] + w > dist[v])
						{
								dist[v] = dist[u] + w;
								prev[v] = u;
								if (!inqueue[v])
								{
										cnt[v]++;
										if (cnt[v] > n) { st.insert(u); cycle = true; continue; }
										q.push(v);
										inqueue[v] = true;
								}
						}
				}
		}
		return !cycle;
}

int n, m;
void DFS(vec<1, pii> adj[], vec<1, bool> &vis, int u)
{
		vis[u] = true;
		for (auto &v : adj[u])
				if (!vis[v.F]) DFS(adj, vis, v.F);
}
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		cin >> n >> m;
		vec<1, pii> adj[n+1];
		while (m--)
		{
				int u, v, w; cin >> u >> v >> w;
				adj[u].push_back({v, w});
		}
		vec<1, int> dist, prev;
		if (spfa(1, n+1, adj, dist, prev)) cout << dist[n] << '\n';
		else
		{
				vec<1, bool> vis(n+1, false);
				for (auto &x : st)
						if (!vis[x]) DFS(adj, vis, x);
				if (vis[1] || vis[n]) cout << "-1\n";
				else cout << dist[n] << '\n';
	}
	return 0;
}
```

### [Flight Discount](https://cses.fi/problemset/task/1195/)

```cpp
#define INF (1LL<<61)
void dijkstra(int s, int n, vector<pii> adj[], vector<int> &dist, vector<int> &prev)
{
		dist.assign(n, INF);
		prev.assign(n, -1);
		dist[s] = 0;
		priority_queue<pii, vector<pii>, greater<pii>> pq;
		pq.push({0, s});
		while (!pq.empty())
		{
				int u = pq.top().second, d = pq.top().first;
				pq.pop();
				if (d != dist[u]) continue;
				for (auto &edge : adj[u])
				{
						int v = edge.first, w = edge.second;
						if (dist[u] + w < dist[v])
						{
								dist[v] = dist[u] + w;
								prev[v] = u;
								pq.push({dist[v], v});
						}
				}
		}
}

signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		vec<1, pii> adj[n+1], adjR[n+1];
		vec<1, tuple<int, int, int>> edges;
		while (m--)
		{
				int u, v, w; cin >> u >> v >> w;
				edges.push_back({u, v, w});
				adj[u].push_back({v, w});
				adjR[v].push_back({u, w});
		}
		vec<1, int> dist1, dist2, prev;
		dijkstra(1, n+1, adj, dist1, prev);
		dijkstra(n, n+1, adjR, dist2, prev);

		int res = dist1[n];
		for (auto &x : edges)
		{
				int u, v, w; tie(u, v, w) = x;
				int cur = dist1[u] + w/2 + dist2[v];
				res = min(res, cur);
		}
		cout << res << '\n';
		return 0;
}
```

### [Cycle Finding](https://cses.fi/problemset/task/1197/)

```cpp
struct edge { int u, v, w; };
#define INF (1LL<<61)
signed main()
{
		ios_base::sync_with_stdio(false); cin.tie(NULL);
		int n, m; cin >> n >> m;
		vec<1, edge> edges(m);
		for (auto &x : edges) cin >> x.u >> x.v >> x.w;

		vec<1, int> dist(n+1, INF), par(n+1, -1);
		int x;
		for (int i = 0; i < n; ++i)
		{
				x = -1;
				for (auto &e : edges)
				{
						if (dist[e.u] + e.w < dist[e.v])
						{
								dist[e.v] = dist[e.u] + e.w;
								par[e.v] = e.u;
								x = e.v;
						}
				}
		}

		if (x == -1) cout << "NO\n";
		else
		{
				cout << "YES\n";
				for (int i = 0; i < n; ++i) x = par[x];
				vec<1, int> cycle;
				for (int v = x; ; v = par[v])
				{
						cycle.push_back(v);
						if (v == x && cycle.size() > 1) break;
				}
				reverse(all(cycle));
				for (auto &x : cycle) cout << x << " ";
				cout << '\n';
		}
		return 0;
}
```

### [Flight Routes](https://cses.fi/problemset/task/1196/)

```cpp
void dijkstraKShortest(int s, int n, int k, vector<pii> adj[], vector<vector<int>> &distance)
{
    int cnt = 0;
    distance.assign(n, vector<int>());
    vector<int> dist(n, 0);
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    pq.push({0, s});
    while (!pq.empty())
    {
        int u = pq.top().second, d = pq.top().first;
        pq.pop();
        if (dist[u] > k) continue;
        distance[u].push_back(d);
        if (u == n-1)
        {
            cnt++;
            if (cnt == k) break;
        }
        dist[u]++;
        for (auto &x : adj[u])
        {
            if (dist[x.first] > k) continue;
            pq.push({d + x.second, x.first});
        }
    }
}
signed main()
{
    ios::sync_with_stdio(0); cin.tie(0);
    int n, m, k; cin >> n >> m >> k;
    vec<1, pii> adj[n+1];
    for (int i = 0; i < m; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
    }
    vector<vector<int>> distance;
    dijkstraKShortest(1, n+1, k, adj, distance);
    for (auto &x : distance[n]) cout << x << " ";
    cout << '\n';
    return 0;
}
```

### [Round Trip II](https://cses.fi/problemset/task/1678/)

```cpp
const int MAXN = 1e5+5;
vec<1, int> adj[MAXN], res;
vec<1, bool> vis(MAXN, false);
int backtrackItem = -1;
bool found = false;
void DFS(int u)
{
    vis[u] = true;
    for (auto &v : adj[u])
    {
        if (!vis[v]) DFS(v);
        else
        {
            res.push_back(v);
            res.push_back(u);
            backtrackItem = v;
            return;
        }
        if (backtrackItem != -1)
        {
            if (!found)
            {
                res.push_back(u);
                if (u == backtrackItem) found = true;
            }
            return;
        }
    }
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
    }
    for (int i = 1; i <= n; ++i)
    {
        if (!vis[i]) DFS(i);
        if (found) break;
        else res.clear();
    }
    if (!found) cout << "IMPOSSIBLE\n";
    else
    {
        reverse(all(res));
        cout << res.size() << '\n';
        for (auto &x : res) cout << x << " ";
        cout << '\n';
    }
    return 0;
}
```

### [Course Schedule](https://cses.fi/problemset/task/1679/)

```cpp
vector<int> topologicalSort(int n, vector<int> adj[])
{
    vector<int> inDeg(n, 0);
    for (int i = 0; i < n; ++i)
        for (auto &x : adj[i]) ++inDeg[x];
    queue<int> q;
    for (int i = 0; i < n; ++i)
        if (inDeg[i] == 0) q.push(i);
    vector<int> topOrder;
    while (!q.empty())
    {
        auto cur = q.front();
        q.pop();
        topOrder.push_back(cur);
        for (auto &x : adj[cur])
        {
            --inDeg[x];
            if (inDeg[x] == 0) q.push(x);
        }
    }
    return topOrder;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, int> adj[n+1];
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u-1].push_back(v-1);
    }
    auto res = topologicalSort(n, adj);
    if (res.size() != n) cout << "IMPOSSIBLE\n";
    else
    {
        for (auto &x : res) cout << x+1 << ' ';
        cout << '\n';
    }
    return 0;
}
```

### [Longest Flight Route](https://cses.fi/problemset/task/1680/)

```cpp
#define INF (1LL<<61)
void zeroOneBFS(int s, int n, vector<pii> adj[], vector<int> &dist, vector<int> &prev)
{
    dist.assign(n, INF);
    prev.assign(n, -1);
    dist[s] = 0;
    deque<int> dq;
    dq.push_front(s);
    while (!dq.empty())
    {
        int u = dq.front();
        dq.pop_front();
        for (auto &edge : adj[u])
        {
            int v = edge.first, w = edge.second;
            if (dist[u] + w < dist[v])
            {
                dist[v] = dist[u] + w;
                prev[v] = u;
                if (w == 1) dq.push_back(v);
                else dq.push_front(v);
            }
        }
    }
}
bool findPath(int from, int to, vector<int> &prev, vector<int> &path)
{
    for (int cur = to; cur != -1; cur = prev[cur])
        path.push_back(cur);
    reverse(path.begin(), path.end());
    return (!path.empty() && *path.begin() == from);
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, pii> adj[n+1];
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u].push_back({v, -1});
    }
    vec<1, int> dist, prev, path;
    zeroOneBFS(1, n+1, adj, dist, prev);
    findPath(1, n, prev, path);
    if (dist[n] == INF) cout << "IMPOSSIBLE\n";
    else
    {
        cout << path.size() << '\n';
        for (auto &x : path) cout << x << " ";
    }
    return 0;
}
```

### [Game Routes](https://cses.fi/problemset/task/1681/)

```cpp
// Naive TLE Solution using BFS
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

    priority_queue<int> pq;
    vec<1, int> cnt(n+1, 0);
    pq.push(1); cnt[1] = 1;
    while (!pq.empty())
    {
        int sz = pq.size();
        while (sz--)
        {
            auto u = pq.top(); pq.pop();
            for (auto &v : adj[u])
            {
                (cnt[v] += 1) %= MOD;
                pq.push(v);
            }
        }
    }
    cout << cnt[n] << '\n';
    return 0;
}

// Accepted Topological Sort solution
vector<int> topologicalSort(int n, vector<int> adj[])
{
    vector<int> inDeg(n, 0);
    for (int i = 0; i < n; ++i)
        for (auto &x : adj[i]) ++inDeg[x];
    queue<int> q;
    for (int i = 0; i < n; ++i)
        if (inDeg[i] == 0) q.push(i);
    vector<int> topOrder;
    while (!q.empty())
    {
        auto cur = q.front();
        q.pop();
        topOrder.push_back(cur);
        for (auto &x : adj[cur])
        {
            --inDeg[x];
            if (inDeg[x] == 0) q.push(x);
        }
    }
    return topOrder;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, int> adj[n], inDeg(n, 0), cnt(n, 0);
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u-1].push_back(v-1);
        inDeg[v-1]++;
    }

    cnt[0] = 1;
    for (auto &u : topologicalSort(n, adj))
        for (auto &v : adj[u]) (cnt[v] += cnt[u]) %= MOD;
    cout << cnt[n-1] << '\n';
    return 0;
}
```

### [Investigation](https://cses.fi/problemset/task/1202/)

```cpp
#define INF (1LL<<61)
void dijkstra(int s, int n, vector<pii> adj[], vector<pii> &dist, vector<int> &prev, vector<int> &mn, vector<int> &mx)
{
    dist.assign(n, make_pair(INF, 0));
    mn.assign(n, INF); mx.assign(n, -INF);
    prev.assign(n, -1); dist[s] = {0, 1}; mn[s] = 0, mx[s] = 0;
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    pq.push({0, s});
    while (!pq.empty())
    {
        int u = pq.top().second, d = pq.top().first;
        pq.pop();
        if (d != dist[u].first) continue;
        for (auto &edge : adj[u])
        {
            int v = edge.first, w = edge.second;
            if (dist[u].first + w == dist[v].first)
            {
                (dist[v].second += dist[u].second) %= MOD;
                mn[v] = min(mn[v], mn[u]+1), mx[v] = max(mx[v], mx[u]+1);
            }
            if (dist[u].first + w < dist[v].first)
            {
                dist[v] = {dist[u].first + w, dist[u].second};
                mn[v] = mn[u]+1, mx[v] = mx[u]+1, prev[v] = u;
                pq.push({dist[v].first, v});
            }
        }
    }
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, pii> adj[n+1];
    while (m--)
    {
        int u, v, w; cin >> u >> v >> w;
        adj[u].push_back({v, w});
    }
    vec<1, pii> dist;
    vec<1, int> prev, mn, mx;
    dijkstra(1, n+1, adj, dist, prev, mn, mx);
    cout << dist[n].first << " " << dist[n].second << " " << mn[n] << " " << mx[n] << '\n';
    return 0;
}
```

### [Planets Queries I](https://cses.fi/problemset/task/1750/)

Refer [https://cses.fi/book/book.pdf](https://cses.fi/book/book.pdf) page 164 successor path

```cpp
const int MAXN = 2*1e5 + 5;
vec<2, int> succ(32, MAXN);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    for (int j = 1; j <= n; ++j) cin >> succ[0][j];
    for (int i = 1; i < 32; ++i)
        for (int j = 1; j <= n; ++j)
            succ[i][j] = succ[i-1][succ[i-1][j]];
 
    while (q--)
    {
        int x, k; cin >> x >> k;
        int i = 0;
        while (k)
        {
            if (k&1) x = succ[i][x];
            k >>= 1;
            ++i;
        }
        cout << x << '\n';
    }
    return 0;
}
```

### [Planets Queries II](https://cses.fi/problemset/task/1160/)

Consider below two example function graphs built in top bottom fashion, the array next to it denotes len for 1 its 3, 2 is 2 and so on it can easily be visualized through top bottom fashion representation.

Now if we go k times succ \(lift\) using our log n approach of previous problem, we should choose optimal value of k here there could be 3 things:

* If y is some bottom successor say in left graph x=1 and y=3 then we need to left k=2 i.e. len\[x\]-len\[y\] times.
* There could be a circular scenerio as well consider right graph if x=4 y=2 then first condition will fail in that case we want x to go back to the initial of the loop here 1 by lifting it len\[x\] times now x=1 y=2 using case 1 will work.
* If above 2 condition fails then return -1

![](../.gitbook/assets/image%20%2864%29.png)

```cpp
const int MAXN = 2*1e5 + 5;
vec<2, int> succ(32, MAXN);
vec<1, bool> vis(MAXN, false);
vec<1, int> len(MAXN, 0);
void DFS(int u)
{
    vis[u] = true;
    int v = succ[0][u];
    if (!vis[v]) DFS(v);
    len[u] = len[v]+1;
}
int lift(int x, int d)
{
    if (d <= 0) return x;
    int i = 0;
    while (d)
    {
        if (d&1) x = succ[i][x];
        d >>= 1;
        ++i;
    }
    return x;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    for (int j = 1; j <= n; ++j) cin >> succ[0][j];
    for (int i = 1; i < 32; ++i)
        for (int j = 1; j <= n; ++j)
            succ[i][j] = succ[i-1][succ[i-1][j]];
 
    for (int i = 1; i <= n; ++i)
        if (!vis[i]) DFS(i);
 
    while (q--)
    {
        int x, y; cin >> x >> y;
        int z = lift(x, len[x]);
        if (lift(x, len[x]-len[y]) == y) cout << (len[x]-len[y]) << '\n';
        else if (lift(z, len[z]-len[y]) == y) cout << (len[x]+len[z]-len[y]) << '\n';
        else cout << "-1\n";
    }
    return 0;
}
```

### [Planets Cycles](https://cses.fi/problemset/task/1751/)

We are running a DFS to work on a component at a time essentially finding len \(our final result\) for that component at a time. To do so we iterate over entire component until we find a loop, now inside if \(vis\[v\]\) we are doing 2 things once we find our loop, those who are inside loop will have cycLen count and for those outside we run another DFS \(opposite one we have prepared a previous DFS just like successor one\).

```cpp
const int MAXN = 2e5 + 5;
#define INF (1LL<<61)
vec<1, int> succ(MAXN), presc[MAXN], len(MAXN, INF);
vec<1, bool> vis(MAXN, false);
intHashTable rec;
void DFS2(int u, int x)
{
    if (!vis[u]) vis[u] = true;
    len[u] = x;
    for (auto &v : presc[u])
        if (len[v] == INF) DFS2(v, x+1);
}
void DFS(int u)
{
    rec.clear();
    for (int v = u, cnt = 0; ; v = succ[v], cnt++)
    {
        int cycLen = cnt-rec[v];
        if (vis[v])
        {
            int v1 = v;
            while (len[v] == INF)
            {
                len[v] = cycLen;
                v = succ[v];
            }
            for (auto &x : presc[v1])
                if (len[x] == INF) DFS2(x, len[v1]+1);
            break;
        }
        vis[v] = true;
        rec[v] = cnt;
    }
}

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    for (int i = 1; i <= n; ++i)
    {
        cin >> succ[i];
        presc[succ[i]].push_back(i);
    }
    for (int i = 1; i <= n; ++i)
        if (!vis[i]) DFS(i);
    for (int i = 1; i <= n; ++i) cout << len[i] << " "; cout << '\n';
    return 0;
}
```

### [Road Reparation](https://cses.fi/problemset/task/1675/)

Simply find MST

```cpp
const int MAXN = 1e5;
int parent[MAXN+1], sz[MAXN+1];
void makeSet(int x) { parent[x] = x, sz[x] = 1; }
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

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, tiii> edges(m);
    for (auto &x : edges) cin >> get<1>(x) >> get<2>(x) >> get<0>(x);
    sort(all(edges));
    for (int i = 1; i <= n; ++i) makeSet(i);
    int cost = 0, edgeCnt = 0;
    for (auto &x : edges)
    {
        int u = get<1>(x), v = get<2>(x), w = get<0>(x);
        if (findSet(u) != findSet(v))
        {
            cost += w, edgeCnt++;
            unionSet(u, v);
        }
    }
    if (edgeCnt == n-1) cout << cost << '\n';
    else cout << "IMPOSSIBLE\n";
    return 0;
}
```

### [Road Construction](https://cses.fi/problemset/task/1676/)

Apply DSU and little bit of maths

```cpp
const int MAXN = 1e5;
int parent[MAXN+1], sz[MAXN+1];
void makeSet(int x) { parent[x] = x, sz[x] = 1; }
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

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    for (int i = 1; i <= n; ++i) makeSet(i);
    int resX = n, resY = 1;
    while (m--)
    {
        int u, v; cin >> u >> v;
        if (findSet(u) != findSet(v))
        {
            int p = sz[findSet(u)], q = sz[findSet(v)];
            resX--, resY = max(resY, p+q);
            unionSet(u, v);
        }
        cout << resX << " " << resY << '\n';
    }
    return 0;
}
```

### [Flight Routes Check](https://cses.fi/problemset/task/1682/)

Kosaraju Strongly connected component

```cpp
const int MAXN = 1e5;
vector<int> adj[MAXN+1], adjRev[MAXN+1], order, component;
vector<bool> used;
int n, m;
void DFS(int u)
{
    used[u] = true;
    for (auto &v : adj[u])
        if (!used[v]) DFS(v);
    order.push_back(u);
}
void DFS2(int u)
{
    used[u] = true;
    component.push_back(u);
    for (auto &v : adjRev[u])
        if (!used[v]) DFS2(v);
}
signed main()
{
    cin >> n >> m;
    while (m--)
    {
        int u, v; cin >> u >> v;
        u--, v--;
        adj[u].push_back(v);
        adjRev[v].push_back(u);
    }
    used.assign(n, false);
    for (int i = 0; i < n; ++i)
        if (!used[i]) DFS(i);
    used.assign(n, false);
    reverse(order.begin(), order.end());
    vec<1, int> res;
    for (auto &v : order)
    {
        if (used[v]) continue;
        DFS2(v);
        if (!component.empty()) res.push_back(component[0]+1);
        if (res.size() >= 2) { cout << "NO\n" << res[1] << " " << res[0] << '\n'; return 0; }
        component.clear();
    }
    cout << "YES\n";
}
```

### [Planets and Kingdoms](https://cses.fi/problemset/task/1683/)

Again simple kosaraju

```cpp
const int MAXN = 1e5;
vector<int> adj[MAXN+1], adjRev[MAXN+1], order, component, res(MAXN);
vector<bool> used;
int n, m;
void DFS(int u)
{
    used[u] = true;
    for (auto &v : adj[u])
        if (!used[v]) DFS(v);
    order.push_back(u);
}
void DFS2(int u)
{
    used[u] = true;
    component.push_back(u);
    for (auto &v : adjRev[u])
        if (!used[v]) DFS2(v);
}
signed main()
{
    cin >> n >> m;
    while (m--)
    {
        int u, v; cin >> u >> v;
        u--, v--;
        adj[u].push_back(v);
        adjRev[v].push_back(u);
    }
    used.assign(n, false);
    for (int i = 0; i < n; ++i)
        if (!used[i]) DFS(i);
    used.assign(n, false);
    reverse(order.begin(), order.end());
    int cnt = 1;
    for (auto &v : order)
    {
        if (used[v]) continue;
        DFS2(v);
        for (auto &x : component) res[x+1] = cnt;
        cnt++;
        component.clear();
    }
    cout << cnt-1 << '\n';
    for (int i = 1; i <= n; ++i) cout << res[i] << " ";
    cout << '\n';
    return 0;
}
```

### [Giant Pizza](https://cses.fi/problemset/task/1684/)

Standard 2SAT problem

```cpp
vector<vector<int>> adj, adjRev;
vector<bool> vis, sol, isTrue;
vector<int> mvStk;
bool noSolution;
int N;
void DFS1(int node)
{
    vis[node] = true;
    for (auto vec : adj[node])
        if (!vis[vec]) DFS1(vec);
    mvStk.push_back(node);
}
void DFS2(int node)
{
    if (isTrue[node]) noSolution = true;
    vis[node] = true;
    isTrue[node^1] = true;
    sol[node/2] = ((node&1) ^ 1);
    for (auto vec : adjRev[node])
        if (!vis[vec]) DFS2(vec);
}
inline int get_node(int x) { return (x > 0) ? (2 * (x-1) + 1) : (2 * (-x-1)); }
int initialize(int _n)
{
    N = _n;
    adj.assign(2*N, vector<int>());
    adjRev.assign(2*N, vector<int>());
    vis.assign(2*N, false);
    sol.assign(N, false);
    isTrue.assign(2*N, false);
    mvStk.reserve(2*N);
}
void addEdge(int p, int q)
{
    int a, b;
    a = get_node(-p); b = get_node(q);
    adj[a].push_back(b);
    adjRev[b].push_back(a);

    a = get_node(-q); b = get_node(p);
    adj[a].push_back(b);
    adjRev[b].push_back(a);
}
pair<bool, vector<bool>> solve2SAT()
{
    fill(vis.begin(), vis.end(), 0);
    for (int i = 0; i < 2*N; ++i)
        if (!vis[i]) DFS1(i);
    noSolution = false;
    fill(vis.begin(), vis.end(), 0);
    for (int i = 2*N - 1; i >= 0; --i)
        if (!vis[mvStk[i]] && !vis[mvStk[i] ^ 1]) DFS2(mvStk[i]);
    return {!noSolution, sol};
}

signed main()
{
    int n, m; cin >> n >> m;
    initialize(m);
    while (n--)
    {
        int a, b; char p, q; cin >> p >> a >> q >> b;
        if (p == '-') a *= -1;
        if (q == '-') b *= -1;
        addEdge(a, b);
    }
    auto res = solve2SAT();
    if (!res.F) cout << "IMPOSSIBLE\n";
    else
    {
        for (auto x : res.S)
            cout << ((x) ? '+' : '-') << ' ';
        cout << '\n';
    }
    return 0;
}
```

### [Coin Collector](https://cses.fi/problemset/task/1686/)

![](../.gitbook/assets/image%20%2877%29.png)

From the given graph we first find all Strongly connected component, because within each SCC we are allowed to freely move anywhere so if we enter any node then all other nodes belonging to that SCC values will add up. In the image 1, 2 belongs to 1st SCC and 3 2nd and 4th 3rd. Now we create a DAG based on original graph property keeping components grouped. Now since it's a DAG we can easily find longest value path using dp.

```cpp
const int MAXN = 1e5+5;
int n, m, numSCC = 0;
vec<1, int> adj[MAXN], adjRev[MAXN], arr(MAXN), topOrder, comp(MAXN, 0), val(MAXN, 0), SCC[MAXN];
vec<1, bool> vis(MAXN, false);

void DFS(int u)
{
    vis[u] = true;
    for (auto &v : adj[u])
        if (!vis[v]) DFS(v);
    topOrder.push_back(u);
}
void DFS2(int u)
{
    vis[u] = true;
    comp[u] = numSCC;
    val[numSCC] += arr[u];
    for (auto &v : adjRev[u])
        if (!vis[v]) DFS2(v);
}
vec<1, int> dp(MAXN, -1);
ll solve(int u)
{
    if (dp[u] != -1) return dp[u];
    int res = 0;
    for (int v : SCC[u]) res = max(res, solve(v));
    return dp[u] = res + val[u];
}

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) cin >> arr[i];
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        adjRev[v].push_back(u);
    }

    // find topological order for kosaraju algorithm
    for (int i = 1; i <= n; ++i)
        if (!vis[i]) DFS(i);
    reverse(all(topOrder));

    // Apply kosaraju algorithm, find for each node which componentNum it belongs to (comp[x]) then val[x]
    // denotes sum of all nodes within that SCC.
    fill(all(vis), false);
    for (auto &x : topOrder)
        if (!vis[x]) numSCC++, DFS2(x);
    
    // Creating a DAG of components
    for (int u = 1; u <= n; ++u)
        for (auto &v : adj[u])
            if (comp[u] != comp[v]) SCC[comp[u]].push_back(comp[v]);
    
    // dp[x] is maximum path we can have starting from that SCC node
    int ans = 0;
    for (int i = 1; i <= numSCC; i++)
        ans = max(ans, solve(i));
    cout << ans;
    return 0;
}
```

### [Mail Delivery](https://cses.fi/problemset/task/1691/)

```cpp
/* Heirholzer's Algorithm to find Euler path in O(V + E) */
const int MAXN = 1e5+5;
struct Edge;
typedef list<Edge>::iterator iter;
vec<1, int> deg(MAXN, 0);
int edgeCnt = 0;
struct Edge
{
    int nextVertex;
    iter reverseEdge;
    Edge(int _nextVertex) : nextVertex(_nextVertex) { }
};
int n;
list<Edge> adj[MAXN+1];
vector<int> path;
void addEdge(int a, int b)
{
    adj[a].push_front(Edge(b));
    iter ita = adj[a].begin();
    adj[b].push_front(Edge(a));
    iter itb = adj[b].begin();
    ita->reverseEdge = itb;
    itb->reverseEdge = ita;
    deg[a]++, deg[b]++;
    edgeCnt++;
}
void helper(int u)
{
    while (adj[u].size() > 0)
    {
        int v = adj[u].front().nextVertex;
        adj[v].erase(adj[u].front().reverseEdge);
        adj[u].pop_front();
        helper(v);
    }
    path.push_back(u);
}
bool findEulerianPathUndirected(int u, int n)
{
    for (auto &x : deg)
        if (x&1) return false;
    helper(u);
    return (path.size() == edgeCnt+1);
}

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    while (m--)
    {
        int u, v; cin >> u >> v;
        addEdge(u, v);
    }
    if (!findEulerianPathUndirected(1, n)) cout << "IMPOSSIBLE\n";
    else
    {
        for (auto &x : path) cout << x << " ";
        cout << '\n';
    }
    return 0;
}
```

### [De Bruijn Sequence](https://cses.fi/problemset/task/1692/)

[https://www.geeksforgeeks.org/de-bruijn-sequence-set-1/](https://www.geeksforgeeks.org/de-bruijn-sequence-set-1/)

```cpp
unordered_set<string> done;
vec<1, int> edges;
void DFS(string node, string &st)
{
    for (int i = 0; i < st.size(); ++i)
    {
        string cur = node+st[i];
        if (done.find(cur) == done.end())
        {
            done.insert(cur);
            DFS(cur.substr(1), st);
            edges.push_back(i);
        }
    }
}
string deBruijn(int n, string st)
{
    done.clear(); edges.clear();
    string startingNode = string (n-1, st[0]);
    DFS(startingNode, st);
    string res;
    int len = pow(st.size(), n);
    for (int i = 0; i < len; ++i) res += st[edges[i]];
    res += startingNode;
    return res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n; cin >> n;
    string st = "01";
    cout << deBruijn(n, st) << '\n';
    return 0;
}
```

### [Teleporters Path](https://cses.fi/problemset/task/1693/)

```cpp
const int MAXN = 1e5+5;
vec<1, int> adj[MAXN], inDeg(MAXN, 0), outDeg(MAXN, 0);
vector<int> findEulerianPathDirected(int u)
{
    vector<int> stack, curr_edge(MAXN), res;
    stack.push_back(u);
    while (!stack.empty())
    {
        u = stack.back();
        stack.pop_back();
        while (curr_edge[u] < (int)adj[u].size())
        {
            stack.push_back(u);
            u = adj[u][curr_edge[u]++];
        }
        res.push_back(u);
    }
    reverse(res.begin(), res.end());
    return res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        inDeg[v]++, outDeg[u]++;
    }
    bool notPossible = !((inDeg[1] > outDeg[1]) && (inDeg[n] < outDeg[n]) ||
        (inDeg[1] < outDeg[1]) && (inDeg[n] > outDeg[n]));
    for (int i = 2; i <= n-1; ++i)
        if (inDeg[i] != outDeg[i]) { notPossible = true; break; }
    if (notPossible) cout << "IMPOSSIBLE\n";
    else
    {
        for (auto &x : findEulerianPathDirected(1))
            cout << x << " ";
        cout << '\n';
    }
    return 0;
}
```

### [Hamiltonian Flights](https://cses.fi/problemset/task/1690/)

DP stores result for vertex and visited elements if ith bit in set in state means ith node is visited.

```cpp
int n, m;
vec<1, int> adj[20];
int dp[20][(1<<20)];
int solve(int u, int state)
{
    if (__builtin_popcount(state) == n) return (u == n-1);
    else if (u == n-1) return 0;    // heuristic to save from TLE
    if (dp[u][state] != 0) return dp[u][state];
    int res = 0;
    for (auto &v : adj[u])
    {
        if (state&(1<<v)) continue;
        (res += solve(v, (state|(1<<v)))) %= MOD;
    }
    return dp[u][state] = res;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    cin >> n >> m;
    while (m--)
    {
        int u, v; cin >> u >> v;
        adj[u-1].push_back(v-1);
    }
    cout << solve(0, (1<<0)) << '\n';
    return 0;
}
```

### [Knight's Tour](https://cses.fi/problemset/task/1689/)

Visit those states first \(via sorting\) which will have more future valid states

```cpp
vec<2, int> mat(9, 9, 0);
const int dx[] = {-2, -2, -1, -1,  1,  1,  2,  2}, dy[] = {-1,  1, -2,  2, -2,  2, -1,  1};
int deg(int x, int y)
{
    int d = 0;
    for (int i = 0; i < 8; ++i)
    {
        int _x = x + dx[i], _y = y + dy[i];
        if (_x >= 1 && _x <= 8 && _y >= 1 && _y <= 8 && mat[_x][_y] == 0) d++;
    }
    return d;
}
bool solve(int x, int y, int cur = 1)
{
    mat[x][y] = cur;
    if (cur == 64) return true;
    vec<1, tiii> pos;
    for (int i = 0; i < 8; ++i)
    {
        int _x = x + dx[i], _y = y + dy[i];
        if (_x >= 1 && _x <= 8 && _y >= 1 && _y <= 8 && mat[_x][_y] == 0)
            pos.push_back({deg(_x, _y), _x, _y});
    }
    sort(all(pos));
    for (auto &v : pos)
        if (solve(get<1>(v), get<2>(v), cur+1)) return true;
    mat[x][y] = 0;
    return false;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    solve(m, n);
    for (int i = 1; i <= 8; ++i)
    {
        for (int j = 1; j <= 8; ++j) cout << mat[i][j] << " ";
        cout << '\n';
    }
    return 0;
}
```

### [Download Speed](https://cses.fi/problemset/task/1694/)

```cpp
struct FlowEdge { int v, u; ll cap, flow = 0; FlowEdge(int v, int u, ll cap) : v(v), u(u), cap(cap) {} };
struct Dinic
{
		const ll FLOW_INF = 1e18;
		int n, m = 0, s, t;
		vector<FlowEdge> edges;	vector<vector<int>> adj;
		vector<int> level, ptr;	queue<int> q;
		Dinic(int n, int s, int t) : n(n), s(s), t(t) { adj.resize(n); level.resize(n); ptr.resize(n); }
		void addEdge(int u, int v, ll cap)
		{
				edges.emplace_back(u, v, cap); edges.emplace_back(v, u, 0);
				adj[u].push_back(m); adj[v].push_back(m+1);
				m += 2;
		}
		bool bfs()
		{
				while (!q.empty())
				{
						int v = q.front(); q.pop();
						for (int id : adj[v])
						{
								if (edges[id].cap - edges[id].flow < 1) continue;
								if (level[edges[id].u] != -1) continue;
								level[edges[id].u] = level[v] + 1;
								q.push(edges[id].u);
						}
				}
				return level[t] != -1;
		}
		ll dfs(int v, ll pushed)
		{
				if (pushed == 0) return 0;
				if (v == t)	return pushed;
				for (int &cid = ptr[v]; cid < (int)adj[v].size(); cid++)
				{
						int id = adj[v][cid], u = edges[id].u;
						if (level[v] + 1 != level[u] || edges[id].cap - edges[id].flow < 1)	continue;
						ll tr = dfs(u, min(pushed, edges[id].cap - edges[id].flow));
						if (tr == 0) continue;
						edges[id].flow += tr; edges[id ^ 1].flow -= tr;
						return tr;
				}
				return 0;
		}
		ll flow()
		{
				ll f = 0;
				while (true)
				{
						fill(level.begin(), level.end(), -1);
						level[s] = 0;
						q.push(s);
						if (!bfs())	break;
						fill(ptr.begin(), ptr.end(), 0);
						while (ll pushed = dfs(s, FLOW_INF)) f += pushed;
				}
				return f;
		}
};
// O(N^2 * E)

signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    Dinic dinic(n+1, 1, n);
    while (m--)
    {
        int u, v, w; cin >> u >> v >> w;
        dinic.addEdge(u, v, w);
    }
    cout << dinic.flow() << '\n';
    return 0;
}
```

### [Police Chase](https://cses.fi/problemset/task/1695/)

```cpp
struct FlowEdge { int v, u; ll cap, flow = 0; FlowEdge(int v, int u, ll cap) : v(v), u(u), cap(cap) {} };
struct Dinic
{
		const ll FLOW_INF = 1e18;
		int n, m = 0, s, t;
		vector<FlowEdge> edges;	vector<vector<int>> adj;
		vector<int> level, ptr;	queue<int> q;
		Dinic(int n, int s, int t) : n(n), s(s), t(t) { adj.resize(n); level.resize(n); ptr.resize(n); }
		void addEdge(int u, int v, ll cap)
		{
				edges.emplace_back(u, v, cap); edges.emplace_back(v, u, 0);
				adj[u].push_back(m); adj[v].push_back(m+1);
				m += 2;
		}
		bool bfs()
		{
				while (!q.empty())
				{
						int v = q.front(); q.pop();
						for (int id : adj[v])
						{
								if (edges[id].cap - edges[id].flow < 1) continue;
								if (level[edges[id].u] != -1) continue;
								level[edges[id].u] = level[v] + 1;
								q.push(edges[id].u);
						}
				}
				return level[t] != -1;
		}
		ll dfs(int v, ll pushed)
		{
				if (pushed == 0) return 0;
				if (v == t)	return pushed;
				for (int &cid = ptr[v]; cid < (int)adj[v].size(); cid++)
				{
						int id = adj[v][cid], u = edges[id].u;
						if (level[v] + 1 != level[u] || edges[id].cap - edges[id].flow < 1)	continue;
						ll tr = dfs(u, min(pushed, edges[id].cap - edges[id].flow));
						if (tr == 0) continue;
						edges[id].flow += tr; edges[id ^ 1].flow -= tr;
						return tr;
				}
				return 0;
		}
		ll flow()
		{
				ll f = 0;
				while (true)
				{
						fill(level.begin(), level.end(), -1);
						level[s] = 0;
						q.push(s);
						if (!bfs())	break;
						fill(ptr.begin(), ptr.end(), 0);
						while (ll pushed = dfs(s, FLOW_INF)) f += pushed;
				}
				return f;
		}
};
// O(N^2 * E)

vec<2, bool> hasEdge(505, 505, false);
vec<1, int> positive, negative;
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    Dinic dinic(n+1, 1, n);
    while (m--)
    {
        int u, v; cin >> u >> v;
        dinic.addEdge(u, v, 1);
				dinic.addEdge(v, u, 1);
				hasEdge[u][v] = true;
				hasEdge[v][u] = true;
    }
		cout << dinic.flow() << '\n';
		for (int i = 1; i <= n; ++i)
		{
				if (dinic.level[i] >= 0) positive.push_back(i);
				else negative.push_back(i);
		}
		for (auto &x : positive)
				for (auto &y : negative)
						if (hasEdge[x][y]) cout << x << " " << y << '\n';
		return 0;
}
```

### [School Dance](https://cses.fi/problemset/task/1696/)

```cpp
class GraphMatching
{
private:
    static bool findMatch(int i, const vector<vector<int>> &inp_w, vector<int> &out_row, vector<int> &out_column, vector<int> &seen)
    {
        // 0 is NO_EDGE here
        if (seen[i]) return false;
        seen[i] = true;
        for (int j = 0; j < inp_w[i].size(); ++j)
        {
            if (inp_w[i][j] != 0 && out_column[j] < 0)
            {
                out_row[i] = j, out_column[j] = i;
                return true;
            }
        }
        for (int j = 0; j < inp_w[i].size(); ++j)
        {
            if (inp_w[i][j] != 0 && out_row[i] != j)
            {
                if (out_column[j] < 0 || findMatch(out_column[j], inp_w, out_row, out_column, seen))
                {
                    out_row[i] = j, out_column[j] = i;
                    return true;
                }
            }
        }
        return false;
    }
    static bool dfs(int n, int N, vector<vector<int>> &inp_con, vector<int> &blossom, vector<int> &vis, vector<int> &out_match)
    {
        vis[n] = 0;
        for (int i = 0; i < N; ++i)
        {
            if (!inp_con[n][i]) continue;
            if (vis[i] == -1)
            {
                vis[i] = 1;
                if (out_match[i] == -1 || dfs(out_match[i], N, inp_con, blossom, vis, out_match))
                {
                    out_match[n] = i, out_match[i] = n;
                    return true;
                }
            }
            if (vis[i] == 0 || blossom.size())
            {
                blossom.push_back(i), blossom.push_back(n);
                if (n == blossom[0])
                {
                    out_match[n] = -1;
                    return true;
                }
                return false;
            }
        }
        return false;
    }
    static bool augment(int N, vector<vector<int>> &inp_con, vector<int> &out_match)
    {
        for (int i = 0; i < N; ++i)
        {
            if (out_match[i] != -1) continue;
            vector<int> blossom;
            vector<int> vis(N, -1);
            if (!dfs(i, N, inp_con, blossom, vis, out_match)) continue;
            if (blossom.size() == 0) return true;
            int base = blossom[0], S = blossom.size();
            vector<vector<int>> newCon = inp_con;
            for (int i = 1; i != S-1; ++i)
                for (int j = 0; j < N; ++j)
                    newCon[base][j] = newCon[j][base] |= inp_con[blossom[i]][j];
            for (int i = 1; i != S-1; ++i)
                for (int j = 0; j < N; ++j)
                    newCon[blossom[i]][j] = newCon[j][blossom[i]] = 0;
            newCon[base][base] = 0;
            if (!augment(N, newCon, out_match)) return false;
            int n = out_match[base];
            if (n != -1)
            {
                for (int i = 0; i < S; ++i)
                {
                    if (!inp_con[blossom[i]][n]) continue;
                    out_match[blossom[i]] = n, out_match[n] = blossom[i];
                    if (i&1)
                    {
                        for (int j = i+1; j < S; j += 2)
                            out_match[blossom[j]] = blossom[j+1], out_match[blossom[j+1]] = blossom[j];
                    }
                    else
                    {
                        for (int j = 0; j < i; j += 2)
                            out_match[blossom[j]] = blossom[j+1], out_match[blossom[j+1]] = blossom[j];
                    }
                    break;
                }
            }
            return true;
        }
        return false;
    }
public:
    /* weighted bipartite matching, inp_w[i][j] is cost from row node i to column node j
    out vector out_row & out_column is assignment for that node or -1 if unassigned
    It has a heuristic that will give excellent performance on complete graphs where rows <= columns. */
    static int bipartiteMatching(const vector<vector<int>> &inp_w, vector<int> &out_row, vector<int> &out_column)
    {
        out_row = vector<int> (inp_w.size(), -1);
        out_column = vector<int> (inp_w[0].size(), -1);
        vector<int> seen(inp_w.size());
        int count = 0;
        for (int i = 0; i < inp_w.size(); ++i)
        {
            fill(seen.begin(), seen.end(), 0);
            if (findMatch(i, inp_w, out_row, out_column, seen)) count++;
        }
        return count;
    }
    // unweighted inp_conn[i][j] = 1 for edge 0 otherwise, returns no. of edges in max matching
    static int nonBipartiteMatching(vector<vector<int>> &inp_con, vector<int> &out_match)
    {
        int res = 0;
        int N = inp_con.size();
        out_match = vector<int>(N, -1);
        while (augment(N, inp_con, out_match)) res++;
        return res;
    }
};

vector<vector<int>> inp_w;
vector<int> out_row, out_column;
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m, k; cin >> n >> m >> k;
    inp_w.assign(n, vector<int>(m, 0));
    while (k--)
    {
        int u, v; cin >> u >> v;
        inp_w[u-1][v-1] = 1;
    }
    cout << GraphMatching::bipartiteMatching(inp_w, out_row, out_column) << '\n';
    for (int i = 0; i < n; ++i)
    {
        if (out_row[i] == -1) continue;
        cout << i+1 << " " << out_row[i]+1 << '\n';
    }
    return 0;
}
```

### [Distinct Routes](https://cses.fi/problemset/task/1711/)

```cpp
struct FlowEdge { int v, u; ll cap, flow = 0; FlowEdge(int v, int u, ll cap) : v(v), u(u), cap(cap) {} };
struct Dinic
{
    const ll FLOW_INF = 1e18;
    int n, m = 0, s, t;
    vector<FlowEdge> edges;	vector<vector<int>> adj;
    vector<int> level, ptr;	queue<int> q;
    Dinic(int n, int s, int t) : n(n), s(s), t(t) { adj.resize(n); level.resize(n); ptr.resize(n); }
    void addEdge(int u, int v, ll cap)
    {
        edges.emplace_back(u, v, cap); edges.emplace_back(v, u, 0);
        adj[u].push_back(m); adj[v].push_back(m+1);
        m += 2;
    }
    bool bfs()
    {
        while (!q.empty())
        {
            int v = q.front(); q.pop();
            for (int id : adj[v])
            {
                if (edges[id].cap - edges[id].flow < 1) continue;
                if (level[edges[id].u] != -1) continue;
                level[edges[id].u] = level[v] + 1;
                q.push(edges[id].u);
            }
        }
        return level[t] != -1;
    }
    ll dfs(int v, ll pushed)
    {
        if (pushed == 0) return 0;
        if (v == t)	return pushed;
        for (int &cid = ptr[v]; cid < (int)adj[v].size(); cid++)
        {
            int id = adj[v][cid], u = edges[id].u;
            if (level[v] + 1 != level[u] || edges[id].cap - edges[id].flow < 1)	continue;
            ll tr = dfs(u, min(pushed, edges[id].cap - edges[id].flow));
            if (tr == 0) continue;
            edges[id].flow += tr; edges[id ^ 1].flow -= tr;
            return tr;
        }
        return 0;
    }
    ll flow()
    {
        ll f = 0;
        while (true)
        {
            fill(level.begin(), level.end(), -1);
            level[s] = 0;
            q.push(s);
            if (!bfs())	break;
            fill(ptr.begin(), ptr.end(), 0);
            while (ll pushed = dfs(s, FLOW_INF)) f += pushed;
        }
        return f;
    }
};
// O(N^2 * E)
// TO FIND MINIMUM CUT: https://cses.fi/problemset/task/1695/ (problem) https://ideone.com/90rVfx (solution)
// TO FIND ALL PATHS OF FLOW: https://cses.fi/problemset/task/1711/ (problem) https://ideone.com/wMvHeK (solution)

vec<2, bool> chosenFlow(505, 505, false);
vec<1, bool> vis(505, false);
vec<1, int> adj[505];
vec<1, int> res;
int n, m;
void DFS(int u)
{
    if (u == n)
    {
        cout << res.size() << '\n';
        for (auto &x : res) cout << x << " ";
        cout << '\n';
        return;
    }
    vis[u] = true;
    for (auto &v : adj[u])
    {
        if (!vis[v] && chosenFlow[u][v])
        {
            res.push_back(v);
            DFS(v);
            res.pop_back();
            chosenFlow[u][v] = false;
        }
    }
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    cin >> n >> m;
    Dinic dinic(n+1, 1, n);
    while (m--)
    {
        int u, v; cin >> u >> v;
        dinic.addEdge(u, v, 1);
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    cout << dinic.flow() << '\n';
    for (auto &x : dinic.edges)
        if (x.flow == -1) chosenFlow[x.u][x.v] = true;
    res.push_back(1);
    DFS(1);
    return 0;
}
```

