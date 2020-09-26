# ADHOC

### Super-Duper Easy

```cpp
// UVA00272
signed main()
{
    string str;
    bool first = true;
    while (getline(cin, str))
    {
        for (auto ch : str)
        {
            if (ch == '\"')
            {
                if (first) cout << "``";
                else cout << "''";
                first = !first;
            }
            else cout << ch;
        }
        cout << '\n';
    }
    return 0;
}

// UVA10550
signed main()
{
    int s, a, b, c;
    while (cin >> s >> a >> b >> c && (s || a || b || c))
        cout << (1080) + ((s-a+40)%40 + (b-a+40)%40 + (b-c+40)%40) * 9 << '\n';
    return 0;
}

// UVA11044
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int x, y;
        cin >> x >> y;
        cout << (x/3) * (y/3) << '\n';
    }
    return 0;
}

// UVA11364
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        vector<int> arr(n);
        for (int i = 0; i < n; ++i) cin >> arr[i];
        cout << (*max_element(arr.begin(), arr.end()) - *min_element(arr.begin(), arr.end())) * 2 << '\n';
    }
    return 0;
}

// UVA11498
signed main()
{
    int n, px, py, x, y;
    cin >> n;
    while (n)
    {
        cin >> px >> py;
        while (n--)
        {
            cin >> x >> y;
            if (x == px || y == py) cout << "divisa\n";
            else if (x < px && y > py) cout << "NO\n";
            else if (x > px && y > py) cout << "NE\n";
            else if (x > px && y < py) cout << "SE\n";
			else if (x < px && y < py) cout << "SO\n";
        }
        cin >> n;
    }
    return 0;
}

// UVA11547
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        int x = 315*n + 36962;
        string y = to_string(x);
        cout << y[y.size()-2] << '\n';
    }
    return 0;
}

// UVA11727
signed main()
{
    int t;
    cin >> t;
    for (int i = 1; i <= t; ++i)
    {
        vector<int> a(3);
        for (int i = 0; i < 3; ++i) cin >> a[i];
        sort(a.begin(), a.end());
        cout << "Case " << i << ": " << a[1] << '\n';
    }
    return 0;
}

// UVA12250
signed main()
{
    string helloArray [] = {"HELLO", "HOLA", "HALLO", "BONJOUR", "CIAO", "ZDRAVSTVUJTE"};
    string languageArray [] = {"ENGLISH", "SPANISH", "GERMAN", "FRENCH", "ITALIAN","RUSSIAN"};
    string inp;
    int cases = 0;
    while (cin >> inp && inp != "#")
    {
        int len = 6;
        bool found = false;
        for (int i = 0; i < len; ++i)
        {
            if (helloArray [i] == inp)
            {
                printf ("Case %d: %s\n", ++cases, languageArray [i].c_str());
                found = true;
                break;
            }
        }
        if (!found) printf ("Case %d: UNKNOWN\n", ++cases);
    }
    return 0;
}

// UVA12279
signed main()
{
    int n;
    int cases = 1;
    while (cin >> n && n)
    {
        int ans = 0;
        for (int i = 0; i < n; ++i)
        {
            int x;
            cin >> x;
            if (x == 0) ans--;
            else ans++;
        }
        cout << "Case " << cases << ": " << ans << '\n';
        cases++;
    }
    return 0;
}

//UVA12289
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        string str;
        cin >> str;
        if (str.size() == 5) cout << "3\n";
        else
        {
            int count = 0;
            if (str[0] == 'o') count++;
            if (str[1] == 'n') count++;
            if (str[2] == 'e') count++;
            if (count >= 2) cout << "1\n";
            else cout << "2\n";
        }
    }
    return 0;
}

// UVA12372
signed main()
{
    int t;
    cin >> t;
    int cases = 1;
    while (t--)
    {
        int x, y, z;
        cin >> x >> y >> z;
        if (x <= 20 && y <= 20 && z <= 20) cout << "Case " << cases << ": good\n";
        else cout << "Case " << cases << ": bad\n";
        ++cases;
    }
    return 0;
}

// UVA12403
signed main()
{
    int t;
    cin >> t;
    int cur = 0;
    while (t--)
    {
        string x;
        int v;
        cin >> x;
        if (x == "donate")
        {
            cin >> v;
            cur += v;
        }
        else cout << cur << '\n';
    }
    return 0;
}
```

### Super-Easy

```cpp
// UVA00621
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        string str;
        cin >> str;
        if (str == "1" || str == "4" || str == "78") cout << "+\n";
        else if (str.size() >= 3 && str[str.size()-2] == '3' && str[str.size()-1] == '5')  cout << "-\n";
        else if (str.size() >= 3 && str[0] == '9' && str[str.size()-1] == '4')  cout << "*\n";
        else cout << "?\n";
    }
    return 0;
}

// UVA10114
signed main()
{
    int d, dep;
    double dp, a;
    while (cin >> d && d >= 0)
    {
        cin >> dp >> a;
        cin >> dep;
        unordered_map<int, double> deprec;
        while (dep--)
        {
            int nx;
            double ny;
            cin >> nx >> ny;
            deprec[nx] = ny;
        }
        double prev = deprec[0];
        for (int i = 1; i <= d; ++i)
        {
            if (deprec[i] == 0) deprec[i] = prev;
            else prev = deprec[i];
        }
        double curVal = (a + dp) * (1 - deprec[0]);
        double curLoan = a;
        int month = 0;
        while (curVal < curLoan)
        {
            month++;
            curLoan -= (a/d);
            curVal = curVal * (1 - deprec[month]);
        }
        cout << month;
        if (month != 1) cout << " months\n";
        else cout << " month\n";
    }
    return 0;
}

// UVA10300
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        int ans = 0;
        for (int i = 0; i < n; ++i)
        {
            int x, y, z;
            cin >> x >> y >> z;
            ans += x*z;
        }
        cout << ans << '\n';
    }
    return 0;
}

// UVA10963
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        int x, y;
        cin >> x >> y;
        int gap = (x-y);
        bool ans = true;
        for (int i = 0; i < n-1; ++i)
        {
            cin >> x >> y;
            if ((x-y) != gap) ans = false;
        }
        if (ans) cout << "yes\n";
        else cout << "no\n";
        if (t) cout << '\n';
    }
    return 0;
}

// UVA11332
signed main()
{
    int n;
    while (cin >> n && n)
    {
        while (n/10)
        {
            int x = 0;
            while (n)
            {
                x += n % 10;
                n /= 10;
            }
            n = x;
        }
        cout << n << '\n';
    }
    return 0;
}

// UVA1559
int n, s, m[30], dp[30][1<<14];
int solve(int d, int s)
{
    if (s <= 0) return 1;
    if (d >= 2*n) d -= 2*n;
    if (dp[d][s] != -1) return dp[d][s];
    dp[d][s] = 0;
    for (int i = 1; i <= m[d]; ++i)
        if (!solve(d+1, s-i)) dp[d][s] = 1;
    return dp[d][s];
}
signed main()
{
    while (cin >> n && n)
    {
        cin >> s;
        for (int i = 0; i < 2*n; ++i) cin >> m[i];
        memset(dp, -1, sizeof(dp));
        cout << solve(0, s) << '\n';
    }
    return 0;
}

// UVA11679
signed main()
{
    int b, n;
    while (cin >> b >> n && (b != 0 || n != 0))
    {
        vector<int> banks(b);
        for (int i = 0; i < b; ++i) cin >> banks[i];
        for (int i = 0; i < n; ++i)
        {
            int deb, cred, val;
            cin >> deb >> cred >> val;
            banks[deb-1] -= val;
            banks[cred-1] += val;
        }
        if (*min_element(banks.begin(), banks.end()) < 0) cout << "N\n";
        else cout << "S\n";
    }
}

// UVA11764
signed main()
{
    int t;
    cin >> t;
    int tc = 0;
    while (t--)
    {
        tc++;
        int n;
        cin >> n;
        int x = 0, y = 0;
        int prev;
        cin >> prev;
        for (int i = 0; i < n-1; ++i)
        {
            int cur;
            cin >> cur;
            if (cur > prev) x++;
            else if (cur < prev) y++;
            prev = cur;
        }
        cout << "Case " << tc << ": " << x << " " << y << '\n';
    }
    return 0;
}

// UVA11942
signed main()
{
    int t;
    cin >> t;
    cout << "Lumberjacks:\n";
    while (t--)
    {
        vector<int> arr(10);
        for (int i = 0; i < 10; ++i) cin >> arr[i];
        vector<int> s1 = arr, s2 = arr;
        sort(s1.begin(), s1.end());
        sort(s2.begin(), s2.end(), greater<int>());
        bool flag = true;
        for (int i = 0; i < 10; ++i)
        {
            if (s1[i] != arr[i])
            {
                flag = false;
                break;
            }
        }
        if (flag) cout << "Ordered\n";
        else
        {
            flag = true;
            for (int i = 0; i < 10; ++i)
            {
                if (s2[i] != arr[i])
                {
                    flag = false;
                    break;
                }
            }
            if (flag) cout << "Ordered\n";
            else cout << "Unordered\n";
        }
    }
    return 0;
}

// UVA12015
signed main()
{
    int t;
    cin >> t;
    int tc = 0;
    while (t--)
    {
        ++tc;
        map<int, vector<string>> rec;
        for (int i = 0; i < 10; ++i)
        {
            string x;
            int y;
            cin >> x >> y;
            rec[-y].push_back(x);
        }
        cout << "Case #" << tc <<":\n";
        for (auto x : rec.begin()->second)
            cout << x << '\n';
    }
    return 0;
}

// UVA12157
signed main()
{
    int t;
    cin >> t;
    int tc = 1;
    while (t--)
    {
        int n;
        cin >> n;
        int x = 0, y = 0;
        while (n--)
        {
            int cur;
            cin >> cur;
            x += ceil(cur/(double)29.9) * 10;
            y += ceil(cur/(double)59.9) * 15;
        }
        cout << "Case " << tc << ": ";
        if (x > y) cout << "Juice " << y << '\n';
        else if (x == y) cout << "Mile Juice " << x << '\n';
        else cout << "Mile " << x << '\n';
        ++tc;
    }
    return 0;
}

// UVA12468
signed main()
{
    int a, b;
    while (cin >> a >> b && (a != -1 || b != -1))
        cout << min((b - a + 100) % 100, (a - b + 100) % 100) << '\n';
    return 0;
}

// UVA12503
signed main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        vector<bool> instr(n); // true = left, false = right
        int pos = 0;
        for (int i = 0; i < n; ++i)
        {
            string x;
            cin >> x;
            if (x == "LEFT") instr[i] = true, pos--;
            else if (x == "RIGHT") instr[i] = false, pos++;
            else
            {
                int num;
                cin >> x;
                cin >> num;
                instr[i] = instr[num-1];
                if (instr[i]) pos--;
                else pos++;
            }
        }
        cout << pos << '\n';
    }
    return 0;
}
```

### Easy

```cpp
// UVA00119
signed main()
{
    int n;
    cin >> n;
    while (true)
    {
        unordered_map<string, int> rec;
        vector<string> rec2(n);
        for (int i = 0; i < n; ++i)
        {
            string name;
            cin >> name;
            rec[name] = i;
            rec2[i] = name;
        }
        vector<pair<int, int>> adj[n];
        for (int i = 0; i < n; ++i)
        {
            string name;
            int amount, num;
            cin >> name >> amount >> num;
            if (num == 0) continue;
            int div = amount/num;
            int from = rec[name];
            while (num--)
            {
                cin >> name;
                adj[from].push_back({rec[name], div});
            }
        }
        int score[n] {};
        for (int x = 0; x < n; ++x)
        {
            for (auto y : adj[x])
            {
                score[x] -= y.second;
                score[y.first] += y.second;
            }
        }
        for (int i = 0; i < n; ++i)
            cout << rec2[i] << " " << score[i] << '\n';
        if (cin >> n) cout << '\n';
        else break;
    }
    return 0;
}

// UVA00573
signed main()
{
    double h, d, n, f;
    while (cin >> h >> d >> n >> f && (h != 0 || d != 0 || n != 0 || f != 0))
    {
        int day = 1;
        double init_d = d, cur = 0;
        while (true)
        {
            cur += (d - n);
            if (d > 0) d -= (f * init_d) / (double)100;
            if (cur+n > h)
            {
                cout << "success on day " << day << '\n';
                break;
            }
            else if (cur < 0)
            {
                cout << "failure on day " << day << '\n';
                break;
            }
            day++;
        }
    }
    return 0;
}

// UVA00661
signed main()
{
    int n, m, c;
    int tc = 1;
    while (cin >> n >> m >> c && (n != 0 || m != 0 || c != 0))
    {
        int arr[n];
        for (int i = 0; i < n; ++i) cin >> arr[i];
        int v = 0, ans = 0;
        cout << "Sequence " << tc << '\n';
        bool res = true;
        while (m--)
        {
            int x;
            cin >> x;
            x--;
            arr[x] *= -1;
            v -= arr[x];
            ans = max(ans, v);
            if (ans > c) res = false;
        }
        if (res)
        {
            cout << "Fuse was not blown.\n";
            cout << "Maximal power consumption was " << ans << " amperes.\n";
        }
        else cout << "Fuse was blown.\n";
        cout << '\n';
        ++tc;
    }
    return 0;
}

// UVA10141
signed main()
{
    int r, n, tc = 0;
    string x;
    while (cin >> r >> n && (r != 0 || n != 0))
    {
        tc++;
        double price = 2000000000, p;
        int reqMet = -1, rm;
        string name, nam;
        for (int i = 0; i < r; i++) cin.ignore(), getline(cin, x);
        while (n--)
        {
            getline(cin, nam);
            scanf("%lf %d\n", &p, &rm);
            if ((rm > reqMet) || (rm == reqMet && p < price))
                reqMet = rm, name = nam, price = p;
            for (int i = 0; i < rm; i++) getline(cin, x);
        }

        if (tc > 1) cout << '\n';
        cout << "RFP #" << tc << '\n';
        cout << name << '\n';
    }

    return 0;
}

// UVA10324
signed main()
{
    string str;
    int tc = 1;
    while (cin >> str && str != "0")
    {
        int arr[str.size()+1];
        arr[0] = 0;
        for (int i = 1; i <= str.size(); ++i)
            arr[i] = arr[i-1] + (str[i-1] - '0');
        int q;
        cin >> q;
        cout << "Case " << tc << ":\n";
        while (q--)
        {
            int x, y;
            cin >> x >> y;
            int p = min(x, y), q = max(x, y);
            if (arr[q+1] - arr[p] == 0 || arr[q+1] - arr[p] == q-p+1) cout << "Yes" << '\n';
            else cout << "No" << '\n';
        }
        ++tc;
    }
    return 0;
}

// UVA10424
signed main()
{
    string a, b;
    // getline takes empty character cin >> a WOULD NOT
    while (getline(cin, a))
    {
        getline(cin, b);
        int s1 = 0, s2 = 0;
        for (int i = 0; i < a.size(); ++i)
        {
            if (a[i] >= 'a' && a[i] <= 'z') s1 += a[i] - 'a' + 1;
            if (a[i] >= 'A' && a[i] <= 'Z') s1 += a[i] - 'A' + 1;
        }
        for (int i = 0; i < b.size(); ++i)
        {
            if (b[i] >= 'a' && b[i] <= 'z') s2 += b[i] - 'a' + 1;
            if (b[i] >= 'A' && b[i] <= 'Z') s2 += b[i] - 'A' + 1;
        }
        while (s1/10)
        {
            int cur = 0;
            while (s1)
            {
                cur += s1%10;
                s1 /= 10;
            }
            s1 = cur;
        }
        while (s2/10)
        {
            int cur = 0;
            while (s2)
            {
                cur += s2%10;
                s2 /= 10;
            }
            s2 = cur;
        }
        double ans = min(((double)s1/s2), ((double)s2/s1));
        cout << fixed << setprecision(2) << ans * 100 << " %\n";
    }
    return 0;
}

// UVA10919
signed main()
{
    int k, m;
    while (cin >> k && k != 0)
    {
        cin >> m;
        map<string, int> courses;
        for (int i = 0; i < k; ++i)
        {
            string x;
            cin >> x;
            ++courses[x];
        }
        bool ans = true;
        for (int i = 0; i < m; ++i)
        {
            int c, r;
            cin >> c >> r;
            for (int j = 0; j < c; ++j)
            {
                string x;
                cin >> x;
                if (courses.find(x) != courses.end()) --r;
            }
            if (r > 0) ans = false;
        }
        cout << (ans ? "yes\n" : "no\n");
    }
    return 0;
}

// UVA11507
signed main()
{
    bends["+z"]["+z"] = "-x",
    bends["+z"]["-z"] = "+x";
    bends["-z"]["+z"] = "+x";
    bends["-z"]["-z"] = "-x";

    bends["+y"]["+y"] = "-x",
    bends["+y"]["-y"] = "+x";
    bends["-y"]["+y"] = "+x";
    bends["-y"]["-y"] = "-x";

    bends["+z"]["+y"] = "+z",
    bends["+z"]["-y"] = "+z";
    bends["-z"]["+y"] = "-z";
    bends["-z"]["-y"] = "-z";

    bends["+y"]["+z"] = "+y",
    bends["+y"]["-z"] = "+y";
    bends["-y"]["+z"] = "-y";
    bends["-y"]["-z"] = "-y";

    bends["+x"]["+y"] = "+y",
    bends["+x"]["-y"] = "-y";
    bends["+x"]["+z"] = "+z",
    bends["+x"]["-z"] = "-z";
    bends["-x"]["+y"] = "-y",
    bends["-x"]["-y"] = "+y";
    bends["-x"]["+z"] = "-z",
    bends["-x"]["-z"] = "+z";

    int n;
    while (cin >> n && n)
    {
        n--;
        string ans = "+x", cur;
        while (n--)
        {
            cin >> cur;
            if (cur != "No") ans = bends[ans][cur];
        }
        cout << ans << '\n';
    }
    return 0;
}

// UVA11586
signed main()
{
    int t;
    cin >> t;
    cin.ignore();
    while (t--)
    {
        string str;
        getline(cin, str);
        stringstream ss(str);
        string temp;
        int maleBegining = 0, femaleBeginning = 0;
        int maleEnd = 0, femaleEnd = 0;
        int n = 0;
        while (getline(ss, temp, ' '))
        {
            if (temp[0] == 'M') maleBegining++;
            else femaleBeginning++;
            if (temp[1] == 'M') maleEnd++;
            else femaleEnd++;
            n++;
        }
        if ((maleBegining == femaleEnd || maleEnd == femaleBeginning) && n%2 == 0) cout << "LOOP\n";
        else cout << "NO LOOP\n";
    }
    return 0;
}

// UVA11661
signed main()
{
    int n;
    while (cin >> n && n)
    {
        string str;
        cin >> str;
        int ans = INT_MAX;
        int prev = -1;
        char prevTyp = 'X';
        bool flag = false;
        for (int i = 0; i < n; ++i)
        {
            if (str[i] == '.') continue;
            if (str[i] == 'Z')
            {
                cout << "0\n";
                flag = true;
                break;
            }
            if (str[i] == 'R')
            {
                if (prevTyp == 'D') ans = min(ans, i-prev);
                prev = i, prevTyp = str[i];
            }
            if (str[i] == 'D')
            {
                if (prevTyp == 'R') ans = min(ans, i-prev);
                prev = i, prevTyp = str[i];
            }
        }
        if (!flag) cout << ans << '\n';
    }
    return 0;
}

// UVA11683
signed main()
{
    int a, c;
    while (cin >> a && a)
    {
        cin >> c;
        vector<int> arr(c);
        for (int i = 0; i < c; ++i) cin >> arr[i];
        int tot, cur;
        for (int i = 0; i < c; ++i)
        {
            if (i == 0) tot = a-arr[i], cur = arr[i];
            else
            {
                if (arr[i] < cur)
                    tot += (cur-arr[i]);
            }
            cur = arr[i];
        }
        cout << tot << '\n';
    }
    return 0;
}

// UVA11687
signed main()
{
    string str;
    while (cin >> str && str != "END")
    {
        int n = str.size();
        if (n == 1 && str[0] == '1') cout << "1\n";
        else
        {
            int x = 1, prev = n;
            while (true)
            {
                x++;
                int nw = to_string(prev).size();
                if (nw == prev) break;
                prev = nw;
            }
            cout << x << '\n';
        }
    }
    return 0;
}

// UVA11956
signed main()
{
    int t, tc = 0;
    cin >> t;
    while (t--)
    {
        ++tc;
        string ins;
        cin >> ins;
        int index = 0, out[105] {};
        for (char ch : ins)
        {
            if (ch == '>') index = (index+1) % 100;
            else if (ch == '<') index = (index-1+100) % 100;
            else if (ch == '+') out[index] = (out[index]+1) % 256;
            else if (ch == '-') out[index] = (out[index]-1+256) % 256;
        }
        cout << "Case " << tc << ": ";
        for (int i = 0; i < 100; ++i)
        {
            // prints value in hex making upper case and appending leading zeroes making size 2
            cout << hex << uppercase << setw(2) << setfill('0') << out[i];
            if (i < 99) cout << ' ';
            else cout << '\n';
        }
    }
    return 0;
}
```

### Game \(Cards\)

```cpp
// UVA00162
deque<string> p1, p2, middle;
bool isPlayer = false;
string pickCard()
{
    string card = (isPlayer) ? p2.front() : p1.front();
    if (isPlayer) p2.pop_front();
    else p1.pop_front();
    middle.push_back(card);
    return card;
}
int getRank(string card)
{
    if (card[1] == 'A') return 4;
    else if (card[1] == 'K') return 3;
    else if (card[1] == 'Q') return 2;
    else if (card[1] == 'J') return 1;
    else return 0;
}
signed main()
{
    string str;
    while (cin >> str && str != "#")
    {
        isPlayer = false;
        p1.clear(), p2.clear(), middle.clear();
        p1.push_front(str);
        for (int i = 1; i < 52; ++i)
        {
            cin >> str;
            if (i&1) p2.push_front(str);
            else p1.push_front(str);
        }
        string prevCard = pickCard();
        isPlayer = !isPlayer;
        int counter = 100;
        while (true)
        {
            int rank = getRank(prevCard);
            if (rank)
            {
                bool countered = false;
                while (rank--)
                {
                    if ((isPlayer && p2.empty()) || (!isPlayer && p1.empty())) goto x;
                    prevCard = pickCard();
                    if (getRank(prevCard))
                    {
                        countered = true;
                        break;
                    }
                }
                if (!countered)
                {
                    if (!isPlayer) p2.insert(p2.end(), middle.begin(), middle.end());
                    else p1.insert(p1.end(), middle.begin(), middle.end());
                    middle.clear();
                }
            }
            else prevCard = pickCard();
            isPlayer = !isPlayer;
            if ((isPlayer && p2.empty()) || (!isPlayer && p1.empty())) break;
        }
        x:;
        if (p1.size() > p2.size()) printf("%d%3d\n", 2, p1.size());
        else printf("%d%3d\n", 1, p2.size());
    }
    return 0;
}

// UVA00462
signed main()
{
    map<char, int> suitConversion;
    suitConversion['S'] = 0, suitConversion['H'] = 1, suitConversion['D'] = 2, suitConversion['C'] = 3;
    char back[4] = {'S', 'H', 'D', 'C'};
    vector<bool> ace(4), king(4), queen(4), jack(4), trump(4);
    vector<int> count(4);
    int points, extraPoints, pos;
    string temp;
    while (cin >> temp)
    {
        for (int i = 0; i < 4; ++i)
        {
            ace[i] = king[i] = queen[i] = jack[i] = trump[i] = false;
            count[i] = 0;
        }
        points = extraPoints = 0;
        for (int i = 0; i < 13; ++i)
        {
            if (i != 0) cin >> temp;
            pos = suitConversion[temp[1]];
            if (temp[0] == 'A') ace[pos] = true, points += 4;
            else if (temp[0] == 'K') king[pos] = true, points += 3;
            else if (temp[0] == 'Q') queen[pos] = true, points += 2;
            else if (temp[0] == 'J') jack[pos] = true, points += 1;
            ++count[pos];
        }
        for (int i = 0; i < 4; ++i)
        {
            if (ace[i]) trump[i] = true;
            if (king[i] && count[i] < 2) --points;
            else if (king[i]) trump[i] = true;
            if (queen[i] && count[i] < 3) --points;
            else if (queen[i]) trump[i] = true;
            if (jack[i] && count[i] < 4) --points;
            if (count[i] == 2) ++extraPoints;
            else if (count[i] < 2) extraPoints += 2;
        }
        if (trump[0] && trump[1] && trump[2] && trump[3] && points >= 16) cout << "BID NO-TRUMP\n";
        else if (points + extraPoints >= 14)
        {
            char output = 'S';
            int maxV = 0;
            for (int i = 1; i < 4; ++i)
                if (count[i] > count[maxV]) maxV = i, output = back[i];
            cout << "BID " << output << '\n';
        }
        else cout << "PASS\n";
    }
    return 0;
}

// UVA00555
struct person { vector<char> cards[4]; };
int cardRank(char card)
{
    if (card == 'T') return 10;
    else if (card == 'J') return 11;
    else if (card == 'Q') return 12;
    else if (card == 'K') return 13;
    else if (card == 'A') return 14;
    else return (card - '0');
}
signed main()
{
    map<char, int> dir; dir['N'] = 0, dir['E'] = 1, dir['S'] = 2, dir['W'] = 3;
    map<int, char> per; per[0] = 'N', per[1] = 'E', per[2] = 'S', per[3] = 'W';
    map<char, int> typ; typ['C'] = 0, typ['D'] = 1, typ['S'] = 2, typ['H'] = 3;
    map<int, char> inf; inf[0] = 'C', inf[1] = 'D', inf[2] = 'S', inf[3] = 'H';
    string str;
    while (cin >> str && str != "#")
    {
        int curPer = dir[str[0]];
        person persons[4];
        cin >> str;
        for (int i = 0; i < str.size(); i += 2)
        {
            curPer = (curPer+1) % 4;
            persons[curPer].cards[typ[str[i]]].push_back(str[i+1]);
        }
        cin >> str;
        for (int i = 0; i < str.size(); i += 2)
        {
            curPer = (curPer+1) % 4;
            persons[curPer].cards[typ[str[i]]].push_back(str[i+1]);
        }
        curPer = dir['S'];
        for (int counter = 0; counter < 4; curPer = (curPer + 1) % 4, counter++)
        {
            cout << per[curPer] << ": ";
            int sz = 0;
            for (int i = 0; i < 4; ++i)
                if (!persons[curPer].cards[i].empty()) sz = max(sz, i);
            for (int i = 0; i < 4; ++i)
            {
                sort(persons[curPer].cards[i].begin(), persons[curPer].cards[i].end(), [](char a, char b){
                    return (cardRank(a) < cardRank(b));
                });
                for (int x = 0; x < persons[curPer].cards[i].size(); ++x)
                {
                    cout << inf[i] << persons[curPer].cards[i][x];
                    if (x != persons[curPer].cards[i].size()-1 || i != sz) cout << " ";
                }
            }
            cout << '\n';
        }
    }

    return 0;
}

// UVA10205
string values[] = {"2", "3", "4", "5", "6", "7", "8", "9", "10", "Jack", "Queen", "King", "Ace"};
string suits[] = {"Clubs", "Diamonds", "Hearts", "Spades"};
string deck[53];
void initDeck()
{
    for (int i = 1; i < 53; i++) deck[i] = values[(i - 1) % 13] + " of " + suits[(i - 1) / 13];
    return;
}
void Map(string SourceDeck[], string TargetDeck[], vector<vector<int>> &ShuffleMaps, int ShuffleNumber)
{
    for (int i = 1; i < 53; i++) TargetDeck[i] = SourceDeck[ShuffleMaps[ShuffleNumber][i]];
}
int main()
{
    int t;
    scanf("%d\n", &t);
    for (int i = 0; i < t; ++i)
    {
        scanf("\n");
        int nShuffles;
        scanf("%d\n", &nShuffles);
        vector<vector<int>> ShuffleMaps;
        ShuffleMaps.push_back(*(new vector<int>));
        for (int j = 1; j <= nShuffles; j++)
        {
            ShuffleMaps.push_back(*(new vector<int>));
            ShuffleMaps[j].push_back(0);
            for (int k = 1; k < 53; k++)
            {
                int T;
                scanf("%d", &T);
                ShuffleMaps[j].push_back(T);
            }
        }

        vector<int> Shuffles;
        char Input[1000];
        scanf("\n");
        while (fgets(Input, 5, stdin) != NULL)
        {
            int T;
            if (Input[0] == '\n') break;
            stringstream s(Input);
            s >> T;
            Shuffles.push_back(T);
        }

        initDeck();
        for (int j = 0; j < Shuffles.size(); j++)
        {
            string TempDeck[53];
            Map(deck, TempDeck, ShuffleMaps, Shuffles[j]);
            for (int k = 1; k < 53; k++) deck[k] = TempDeck[k];
        }
        for (int k = 1; k < 53; k++) cout << deck[k] << '\n';
        if (i != t - 1) cout << '\n';
    }
    return 0;
}

// UVA10646
int main()
{
    int t;
    cin >> t;
    string pile[27], hand[25];
    for (int tc = 1; tc <= t; ++tc)
    {
        for (int i = 0; i < 27; ++i) cin >> pile[i];
        for (int i = 0; i < 25; ++i) cin >> hand[i];
        int x, y = 0, temp = 27;
        for (int i = 0; i < 3; ++i)
        {
            x = isdigit(pile[temp-1][0]) ? pile[temp-1][0] - '0' : 10;
            y += x;
            temp -= 11-x;
        }
        string p = (y > temp) ? hand[y-1-temp] : pile[y-1];
        cout << "Case "<< tc << ": " << p << '\n';
    }
    return 0;
}

// UVA11678
int main()
{
    int a, b;
    while (cin >> a >> b && (a != 0 || b != 0))
    {
        vector<int> alice(a), bob(b);
        unordered_set<int> rec1, rec2;
        for (int i = 0; i < a; ++i)
        {
            cin >> alice[i];
            rec1.insert(alice[i]);
        }
        for (int i = 0; i < b; ++i)
        {
            cin >> bob[i];
            rec2.insert(bob[i]);
        }
        int p = 0, q = 0;
        for (auto i : rec1)
            if (rec2.find(i) == rec2.end()) q++;
        for (auto i : rec2)
            if (rec1.find(i) == rec1.end()) p++;
        cout << min(p, q) << '\n';
    }
    return 0;
}

// UVA12247
int main()
{
    int a, b, c, d, e, f;
    while (cin >> a >> b >> c >> d >> e && (a != 0 || b != 0 || c != 0 || d != 0 | e != 0))
    {
        if (d > max({a, b, c}) && e > max({a, b, c}))
        {
            int f = 1;
            while (f == d || f == e || f == a || f == b || f == c) f++;
            if (f > 52) cout << "-1\n";
            else cout << f << '\n';
        }
        else if ((d > a && d > b && d > c) || (e > a && e > b && e > c) ||
            (d > b && e > b && d > c && e > c) ||
            (d > a && e > a && d > c && e > c) ||
            (d > a && e > a && d > b && e > b))
        {
            int count = 0;
            if (d > a && d > b && d > c) count = (e < a) + (e < b) + (e < c);
            else if (e > a && e > b && e > c) count = (d < a) + (d < b) + (d < c);
            else count = ((d < a) || (e < a)) + ((d < b) || (e < b)) + ((d < c) || (e < c));
            int f = max({a, b, c});
            if (count == 1)
            {
                int pos = 0;
                if (d > a && d > b && d > c)
                {
                    if (e < a) pos = 0;
                    else if (e < b) pos = 1;
                    else if (e < c) pos = 2;
                }
                else
                {
                    if (d < a) pos = 0;
                    else if (d < b) pos = 1;
                    else if (d < c) pos = 2;
                }
                if (pos == 0) f = max({b, c});
                else if (pos == 1) f = max({a, c});
                else if (pos == 2) f = max({a, b});
            }
            while (f == d || f == e || f == a || f == b || f == c) f++;
            if (f > 52) cout << "-1\n";
            else cout << f << '\n';
        }
        else cout << "-1\n";
    }
    return 0;
}
```

### Game \(Chess\)

```cpp
// UVA00255
char board[8][8];
const int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, -1, 0, 1};
void putPieces(int king, int queen) { board[king/8][king%8] = 'K', board[queen/8][queen%8] = 'Q'; }
bool isValid(int x, int y) { return (x >= 0 && y >= 0 && x < 8 && y < 8); }
void calculateMoves(int king, int queen)
{
	int kx = king/8, ky = king%8, qx = queen/8, qy = queen%8;
	for (int i = 0; i < 4; ++i)
	{
		int nkx = kx + dx[i], nky = ky + dy[i];
		if (isValid(nkx, nky) && board[nkx][nky] != 'Q') board[nkx][nky] = 'k';
	}
	for (int i = 0; i < 4; ++i)
	{
		for (int nqx = qx + dx[i], nqy = qy + dy[i]; ; nqx += dx[i], nqy += dy[i])
		{
			if (isValid(nqx, nqy) && board[nqx][nqy] != 'K')
			{
				if (board[nqx][nqy] == 'k') board[nqx][nqy] = '_';
				else board[nqx][nqy] = 'q';
			} else break;
		}
	}
}
int main()
{
	int k, q, qm;
	while (cin >> k >> q >> qm)
	{
		if (k == q) cout << "Illegal state\n";
		else
		{
			memset(board, 0, 64);
			putPieces(k, q);
			calculateMoves(k, q);
			if (board[qm / 8][qm % 8] == '_') cout << "Move not allowed\n";
			else if (board[qm / 8][qm % 8] != 'q') cout << "Illegal move\n";
			else
			{
				memset(board, 0, 64);
				putPieces(k, qm);
				calculateMoves(k, qm);
				int kingMoves = 0;
				for (int i = 0; i < 8; ++i)
					for (int j = 0; j < 8; ++j)
						kingMoves += (board[i][j] == 'k');
				if (kingMoves > 0) cout << "Continue\n";
				else cout << "Stop\n";
			}
		}
	}
	return 0;
}

// UVA00278
int main()
{
	int t;
	cin >> t;
	while (t--)
	{
		char ch;
		int n, m;
		cin >> ch >> n >> m;
		if (ch == 'r') cout << min(n, m) << '\n';
		else if (ch == 'Q') cout << min(n, m) << '\n';
		else if (ch == 'K') cout << ((m+1)/2 * (n+1)/2) << '\n';
		else if (ch == 'k')
		{
			int ans = 0;
			if (min(m, n) > 0)
			{
				if (min(m, n) == 1) ans = max(m, n);
				else if (min(m, n) == 2)
				{
					int x = max(m, n);
					ans = 4*(x/4) + 2*min((x%4), 2);
				}
				else ans = (((m+1)/2) * ((n+1)/2)) + ((m/2) * (n/2));
			}
			cout << ans << '\n';
		}
	}
	return 0;
}

// UVA10849
int main()
{
	int t;
	cin >> t;
	while (t--)
	{
		int a, b, c, d, n;
		cin >> n >> c;
		while (n--)
		{
			cin >> a >> b >> c >> d;
			if (a == c && b == d) cout << "0\n";
			else if ((a+b)%2 != (c+d)%2) cout << "no move\n";
			else if (a+b == c+d || a-b == c-d) cout << "1\n";
			else cout << "2\n";
		}
	}
	return 0;
}

// UVA11494
int main()
{
	int a, b, c, d;
	while (cin >> a >> b >> c >> d && (a || b || c || d))
	{
		if (a == c && b == d) cout << "0\n";
		// a-b (no abs) is primary diagonal
		// a+b is other diagonal
		else if ((a+b == c+d) || ((a-b) == (c-d)) || a == c || b == d) cout << "1\n";
		else cout << "2\n";
	}
	return 0;
}
```

### Game \(Others\)

```cpp
// UVA00489
int main()
{
	int n;
	while (cin >> n && n != -1)
	{
		string a, b;
		cin >> a >> b;
		set<char> chars;
		for (char ch : a) chars.insert(ch);
		int count = 0;
		bool ended = false;
		cout << "Round " << n << '\n';
		set<char> rec;
		for (int i = 0; i <= b.size(); ++i)
		{
			if (chars.empty())
			{
				cout << "You win.\n";
				ended = true;
				break;
			}
			if (count >= 7)
			{
				cout << "You lose.\n";
				ended = true;
				break;
			}
			if (i < b.size())
			{
				if (rec.find(b[i]) != rec.end()) continue;
				if (chars.find(b[i]) == chars.end()) count++;
				else chars.erase(b[i]);
				rec.insert(b[i]);
			}
		}
		if (!ended) cout << "You chickened out.\n";
	}
	return 0;
}

// UVA00947
void solve(vector<int> &arr, vector<int> &cur, int &count, int x, int y, int pos = 0)
{
	if (pos == cur.size())
	{
		int count_x = 0, count_y = 0;
		map<int, int> elems;
		for (int i = 0; i < cur.size(); ++i)
		{
			if (arr[i] == cur[i]) count_x++;
			else elems[cur[i]]++;
		}
		for (int i = 0; i < cur.size(); ++i)
		{
			if (arr[i] != cur[i])
			{
				if (elems.find(arr[i]) != elems.end())
				{
					count_y++;
					elems[arr[i]]--;
					if (elems[arr[i]] == 0) elems.erase(arr[i]);
				}
			}
		}
		if (count_x == x && count_y == y) count++;
		return;
	}
	for (int i = 1; i <= 9; ++i)
	{
		cur[pos] = i;
		solve(arr, cur, count, x, y, pos+1);
	}
}
int main()
{
	int t;
	cin >> t;
	while (t--)
	{
		int x, y, count = 0;
		string str;
		cin >> str >> x >> y;
		vector<int> arr(str.size());
		for (int i = 0; i < str.size(); ++i) arr[i] = (str[i] - '0');
		vector<int> cur(arr.size());
		solve(arr, cur, count, x, y);
		cout << count << '\n';
	}
	return 0;
}

// UVA10530
int main()
{
	int n;
	string str;
	int l = 1, r = 10;
	while (cin >> n && n)
	{
		cin.ignore();
		getline(cin, str);		// this min max part caused me 1 WA
		if (str == "too high") r = min(r, n-1);
		else if (str == "too low") l = max(l, n+1);
		else if (str == "right on")
		{
			if (n >= l && n <= r) cout << "Stan may be honest\n";
			else cout << "Stan is dishonest\n";
			l = 1, r = 10;
		}
	}
	return 0;
}

// UVA12239
int main()
{
	int n, b, x[100];
	while (cin >> n >> b && (n || b))
	{
		set<int> checker;
		for (int i = 0; i < b; ++i) cin >> x[i];
		for (int i = 0; i < b; ++i)
			for (int j = i; j < b; ++j)
				if (abs(x[i]-x[j]) <= n) checker.insert(abs(x[i]-x[j]));
		if (checker.size() == n+1) cout << "Y\n";
		else cout << "N\n";
	}
	return 0;
}
```

### Game \(Others\) - Hard

```cpp
// UVA00141
#define MOD 923673411
int board[50][50];
char b[500][50][50];
int h[500];
int n;
void copy_p(int p)
{
    int v = 0;
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            b[p][i][j] = board[i][j], v += (v*2 + board[i][j]) % MOD;
    h[p] = v;
}
bool compare(int p, int q)
{
    if (h[p] != h[q]) return false;
    return true;
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            if (b[p][i][j] != b[q][i][j]) return false;
    return false;
}
void rotate(int p, int q)
{
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            b[q][n-j-1][i] = b[p][i][j];
    int v = 0;
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            v = (v*2 + b[q][i][j]) % MOD;
    h[q] = v;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    while (true)
    {
        if (cin >> n && n == 0) break;
        for (int x = 0; x < n; ++x)
            for (int y = 0; y < n; ++y)
                board[x][y] = 0;
        int p = 0;
        copy_p(p++);
        int x, y, i; string s;
        for (i = 0; i < 2*n; ++i)
        {
            cin >> x >> y >> s;
            x--, y--;
            if (s[0] == '+') board[x][y] = 1;
            else if (s[0] == '-') board[x][y] = 0;
            copy_p(p);
            for (int j = 0; j < p; ++j) if (compare(j, p)) goto x;
            rotate(p, p+1); rotate(p+1, p+2); rotate(p+2, p+3);
            p += 4;
        }
        cout << "Draw\n";
        goto y;
        x:;
        cout << "Player " << (((i+1)&1)+1) << " wins on move " << i+1 << '\n';
        for (i++; i < 2*n; ++i) cin >> x >> y >> s;
        y:;
    }
    return 0;
}

// UVA00339

// UVA00379

// UVA00584

// UVA00647

// UVA10363
bool hasNotWon(vector<string> &grid, char ch)
{
    if (grid[0][0] == ch && grid[0][1] == ch && grid[0][2] == ch) return false;
    if (grid[1][0] == ch && grid[1][1] == ch && grid[1][2] == ch) return false;
    if (grid[2][0] == ch && grid[2][1] == ch && grid[2][2] == ch) return false;
    if (grid[0][0] == ch && grid[1][0] == ch && grid[2][0] == ch) return false;
    if (grid[0][1] == ch && grid[1][1] == ch && grid[2][1] == ch) return false;
    if (grid[0][2] == ch && grid[1][2] == ch && grid[2][2] == ch) return false;
    if (grid[0][0] == ch && grid[1][1] == ch && grid[2][2] == ch) return false;
    if (grid[0][2] == ch && grid[1][1] == ch && grid[2][0] == ch) return false;
    return true;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t;
    cin >> t;
    vector<string> grid(3);
    while (t--)
    {
        int xcount = 0, ocount = 0;
        for (int i = 0; i < 3; ++i)
        {
            cin >> grid[i];
            for (char ch : grid[i])
            {
                if (ch == 'X') ++xcount;
                else if (ch == 'O') ++ocount;
            }
        }
        bool valid = false;
        if (xcount == ocount) valid = hasNotWon(grid, 'X');
        else if (xcount == ocount+1) valid = hasNotWon(grid, 'O');
        else valid = false;

        if (valid) cout << "yes\n";
        else cout << "no\n";
    }
    return 0;
}

// UVA10443


// UVA10813
bool bingo[5][5];
int call[75], card[5][5];
bool check()
{
    bool flag;
    for (int i = 0; i < 5; ++i)
    {
        flag = true;
        for (int j = 0; j < 5; ++j)
            if (!bingo[i][j]) { flag = false; break; }
        if (flag) return true;
    }

    for (int i = 0; i < 5; ++i)
    {
        flag = true;
        for (int j = 0; j < 5; ++j)
            if (!bingo[j][i]) { flag = false; break; }
        if (flag) return true;
    }

    flag = true;
    for (int i = 0; i < 5; ++i)
        if (!bingo[i][i]) { flag = false; break; }
    if (flag) return true;

    flag = true;
    for (int i = 0; i < 5; ++i)
        if (!bingo[4-i][i]) { flag = false; break; }
    if (flag) return true;

    return false;
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n;
    cin >> n;
    while (n--)
    {
        int hit = -1;
        for (int i = 0; i < 5; ++i)
        {
            for (int j = 0; j < 5; ++j)
            {
                if (i != 2 || j != 2) cin >> card[i][j];
                bingo[i][j] = false;
            }
        }
        card[2][2] = -1; bingo[2][2] = true;
        for (int i = 0; i < 75; ++i)
        {
            cin >> call[i];
            if (hit < 0)
            {
                for (int j = 0; j < 5; ++j)
                    for (int k = 0; k < 5; ++k)
                        if (call[i] == card[j][k]) bingo[j][k] = true;
                if (check()) hit = i;
            }
        }
        cout << "BINGO after " << hit+1 << " numbers announced" << '\n';
    }
    return 0;
}

// UVA10903
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, k;
    map<string, string> mp;
    mp["rock"] = "paper"; mp["paper"] = "scissors"; mp["scissors"] = "rock";
    bool first = true;
    while (cin >> n && n)
    {
        cin >> k;
        if (!first) cout << '\n';
        k = k * n * (n-1) / 2;
        first = false;
        string p, q; int x, y;
        map<int, int> wins;
        map<int, int> loses;
        set<int> players;
        while (k--)
        {
            cin >> x >> p >> y >> q;
            players.insert(x); players.insert(y);
            if (mp[p] == q) wins[y]++, loses[x]++;
            else if (mp[q] == p) wins[x]++, loses[y]++;
        }
        for (auto x : players)
        {
            double deno = wins[x] + loses[x];
            if (deno == 0) cout << "-\n";
            else cout << fixed << setprecision(3) << (wins[x] / deno) << '\n';
        }
    }
    return 0;
}
```

### Palindromes

```cpp
// UVA00353
bool checkPalindrome(string str)
{
	for (int i = 0, j = str.size()-1; i <= j; ++i, --j)
		if (str[i] != str[j]) return false;
	return true;
}
int main()
{
	string str;
	while (cin >> str)
	{
		set<string> counter;
		for (int i = 1; i <= str.size(); ++i)
		{
			for (int j = 0; j <= str.size()-i; ++j)
			{
				string substr = str.substr(j, i);
				if (checkPalindrome(substr)) counter.insert(substr);
			}
		}
		cout << "The string '" << str << "' contains " << counter.size() << " palindromes." << '\n';
	}
	return 0;
}

// UVA00401
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    map<char, char> mp;
    mp['A'] = 'A'; mp['E'] = '3'; mp['H'] = 'H'; mp['I'] = 'I'; mp['J'] = 'L';
    mp['L'] = 'J'; mp['M'] = 'M'; mp['O'] = 'O'; mp['S'] = '2'; mp['T'] = 'T';
    mp['U'] = 'U'; mp['V'] = 'V'; mp['W'] = 'W'; mp['X'] = 'X'; mp['Y'] = 'Y';
    mp['Z'] = '5'; mp['1'] = '1'; mp['2'] = '5'; mp['3'] = 'E'; mp['5'] = 'Z';
    mp['8'] = '8';
    string str;
    while (cin >> str)
    {
        string mirror = str;
        bool mirrored = true;
        for (char &ch : mirror)
        {
            if (mp.find(ch) == mp.end()) { mirrored = false; break; }
            else ch = mp[ch];
        }
        if (mirrored)
        {
            reverse(all(mirror));
            for (int i = 0; i < str.size(); ++i)
            {
                if (mirror[i] == str[i]) continue;
                else if ((mirror[i] == 'S' || mirror[i] == '5') && (str[i] == 'S' || str[i] == '5')) continue;
                else if ((mirror[i] == '0' || mirror[i] == 'O') && (str[i] == '0' || str[i] == 'O')) continue;
                else { mirrored = false; break; }
            }
        }

        string rev = str;
        reverse(all(rev));
        bool palindrome = (rev == str);
        if (mirrored && palindrome) cout << str << " -- is a mirrored palindrome.\n";
        else if (mirrored && !palindrome) cout << str << " -- is a mirrored string.\n";
        else if (!mirrored && palindrome) cout << str << " -- is a regular palindrome.\n";
        else if (!mirrored && !palindrome) cout << str << " -- is not a palindrome.\n";
        cout << '\n';
    }
    return 0;
}

// UVA10018
pair<int, int> solve(int n, int i = 1)
{
    int a = n, b = 0;
    while (n) b = ((b * 10) + (n % 10)), n /= 10;
    a += b;
    string res = to_string(a);
    string rev = res;
    reverse(all(rev));
    if (rev == res) return {a, i};
    else return solve(a, i+1);
}
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t, x;
    cin >> t;
    while (t--)
    {
        cin >> x;
        auto ans = solve(x);
        cout << ans.second << " " << ans.first << '\n';
    }
    return 0;
}

// UVA11221
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int t;
    cin >> t;
    cin.ignore();
    for (int tc = 1; tc <= t; ++tc)
    {
        string str;
        getline(cin, str);
        stringstream ss;
        for (char &ch : str)
        {
            if ((ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z'))
                ss << ch;
        }
        str = ss.str();
        int n = str.size();
        int k = sqrt(n);
        cout << "Case #" << tc << ":\n";
        if (k * k != n) cout << "No magic :(\n";
        else
        {
            bool flag = true;
            for (int i = 0, j = n-1; i <= j; ++i, --j)
                if (str[i] != str[j]) { flag = false; break; }
            if (!flag) cout << "No magic :(\n";
            else
            {
                for (int i = 0, j = n-1; i <= j; i += k, j -= k)
                    if (str[i] != str[j]) { flag = false; break; }
                if (!flag) cout << "No magic :(\n";
                else cout << k << '\n';
            }
        }
    }
    return 0;
}

// UVA11309
```

### Anagrams

```cpp
// UVA00148

// UVA00156

// UVA00195

// UVA00454

// UVA00630

// UVA00642

// UVA10098
```

### Real Life Problems - Easy

```cpp
// UVA00161

// UVA00187

// UVA00362

// UVA00637

// UVA00857

// UVA10082

// UVA10191

// UVA10528

// UVA10554

// UVA10812

// UVA11530

// UVA11945

// UVA11984

// UVA12195

// UVA12555
```

### Real Life Problems - Hard

```cpp
// UVA00139

// UVA00145

// UVA00333

// UVA00346

// UVA00403

// UVA00447

// UVA00448

// UVA00449

// UVA00457

// UVA00538

// UVA00608

// UVA00706

// UVA01061

// UVA10415

// UVA10659

// UVA11223

// UVA11743

// UVA12342
```

### Time

```cpp
// UVA00170

// UVA00300

// UVA00579

// UVA00893

// UVA10070

// UVA10339

// UVA10371

// UVA10683

// UVA11219

// UVA11356

// UVA11650

// UVA11677

// UVA11947

// UVA11958

// UVA12019

// UVA12136

// UVA12148

// UVA12439

// UVA12531
```

