# String Algorithms

## String Stream

> Strings are mutable in C++, but immutable in java. So it is better to use std::string only in c++, while stringstream is better for java

Improves string concatenation, if say there's an empty string and we do str += "a" in a loop till n times. It's time complexity will be O\(N^2\) and not O\(N\) it's because inside the loop every time a new string will be created while copying previous one so it will be O\(1 + 2 + 3 + ... + n\) = O\(n \* \(n + 1\) / 2\)

```cpp
stringstream ss("initialise with some str");
ss << "append";
cout << ss.str() << endl;

// Split using string stream
stringstream ss("abc def ghi");
string temp;
while (getline(ss, temp, ' ')) cout << temp << endl;
/*
    Output:
    abc
    def
    ghi
*/
```

### [Validate IP Address](https://leetcode.com/problems/validate-ip-address/)

```cpp
class Solution {
public:
    string validIPAddress(string IP)
    {
        stringstream ss(IP);
        string tmp;
        int cnt = 0;
        if (count(IP.begin(), IP.end(), ':') == 7)  // IPv6
        {
            while (getline(ss, tmp, ':'))
            {
                cnt++;
                if (tmp.size() == 0 || tmp.size() > 4 || tmp.find_first_not_of("0123456789abcdefABCDEF") != string::npos)
                    return "Neither";
            }
            return (cnt == 8 ? "IPv6" : "Neither");
        }
        else if (count(IP.begin(), IP.end(), '.') == 3)     // IPv4
        {
            while (getline(ss, tmp, '.'))
            {
                cnt++;
                if (tmp.size() == 0 || tmp.size() > 3 || (tmp[0] == '0' && tmp.size() > 1) || (tmp.find_first_not_of("0123456789") != string::npos) or (stoi(tmp) > 255))
                    return "Neither";
            }
            return (cnt == 4 ? "IPv4" : "Neither");
        }
        else return "Neither";
    }
};
```

### [Remove Comments](https://leetcode.com/problems/remove-comments/)

```cpp
vector<string> removeComments(vector<string>& source)
{
    string file = "", parsedFile = "";
    for (const string line : source) file += (line + '\n');
    for (int i = 0; i < file.size();)
    {
        if (i < file.size()-1 && file[i] == '/' && file[i+1] == '*')
        {
            int j = i+2;
            for(; j < file.size(); j++)
                if(file[j] == '*' && file[j+1] =='/') break;
            i = j+2;
        }
        else if (i < file.size()-1 && file[i] =='/' && file[i+1] =='/')
        {
            int j = i+2;
            for(; j < file.size(); j++) if(file[j] == '\n') break;
            i = j;
        }
        else parsedFile += file[i++];
    }
    stringstream ss(parsedFile); string line;
    vector<string> res;
    while(getline(ss, line)) if(!line.empty()) res.push_back(line);
    return res;
}
```

### [Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

```cpp
class Solution {
public:
    /*
        - one word can occur multiple time in words? (here yes)
        - sliding window technique for each word
        edge case: "aaaaaaaa" ["aa","aa","aa"]
        output should be: [0, 1, 2]
    */
    vector<int> findSubstring(string s, vector<string> &words)
    {
        vector<int> ans;
        int n = s.size(), m = words.size();
        if (n == 0 || m == 0 || n < m*words[0].size()) return ans;
        int k = words[0].size();

        unordered_map<string, int> cnt;
        for (const string x : words) cnt[x]++;
        /* for s = "barfoothefoobarman"
        str will give: bar foo the foo bar man [NEXT LINE] arf oot ... */
        for (int offset = 0; offset < k; ++offset)
        {
            unordered_map<string, int> curCnt;
            int found = 0;
            for (int l = offset, r = offset; r <= n-k; r += k)
            {
                string str = s.substr(r, k);
                if (cnt.find(str) != cnt.end())
                {
                    curCnt[str]++, found++;
                    while (curCnt[str] > cnt[str])
                    {
                        curCnt[s.substr(l, k)]--;
                        found--, l += k;
                    }
                    if (found == m)
                    {
                        ans.push_back(l);
                        curCnt[s.substr(l, k)]--;
                        found--, l += k;
                    }
                }
                else
                {
                    curCnt.clear();
                    found = 0, l = r+k;
                }
            }
        }
        return ans;
    }
};
```

### Minimum Window Problem

Given string S and T, find minimum substring window in S that contains all character of T  
[https://leetcode.com/problems/minimum-window-substring/](https://leetcode.com/problems/minimum-window-substring/)

```cpp
string minWindow(string s, string t)
{
    if (s.size() < t.size()) return "";
    unordered_map<char, int> cnt, curCnt;
    for (const char ch : t) cnt[ch]++;

    int counter = 0, foundMin = INT_MAX, pos;
    for (int l = 0, r = 0; r < s.size(); ++r)
    {
        if (curCnt[s[r]] < cnt[s[r]]) counter++;
        curCnt[s[r]]++;
        while (curCnt[s[l]] > cnt[s[l]]) { curCnt[s[l]]--; l++; }
        if (counter == t.size())
        {
            int newLen = r-l+1;
            if (newLen < foundMin) foundMin = newLen, pos = l;
        }
    }
    return (foundMin == INT_MAX) ? "" : s.substr(pos, foundMin);
}
```

That contains all characters of T in the same order \(basically subsequence of it is T\)

```cpp
/* dp can be represented with 2 states - i (current pos we are at in s) and j
(current pos we are at in t) dp[i][j] means minimum length required if we
start at pos i and j.

transitioning is very easy. INF (1<<30) is uesd instead of INT_MAX to avoid
overflow due to addition operation.

This is still O(NK * N) which will fail for larger test cases. we can optimize
transition part */

vector<vector<int>> dp;
#define INF (1<<30)
int solve(string &s, string &t, int i = 0, int j = 0)
{
    if (j == t.size()) return 0;
    if (i == s.size()) return INF;
    if (dp[i][j] != -1) return dp[i][j];

    int res = INF;
    for (int x = i; x < s.size(); ++x)
    {
        if (s[x] == t[j])
            res = min(res, (x - i + 1) + solve(s, t, x+1, j+1));
    }
    return dp[i][j] = res;
}
string minWindow(string &s, string &t)
{
    dp = vector<vector<int>>(s.size(), vector<int>(t.size(), -1));
    int res = INF, pos;
    for (int i = 0; i < s.size(); ++i)
    {
        int cur = solve(s, t, i);
        if (cur < res) res = cur, pos = i;
    }
    
    return (res == INF) ? "" : s.substr(pos, res);
}

/* transition optimisation - we want to find next position from i which has
a specific character, using set lower bound stuff */
vector<vector<int>> dp;
unordered_map<char, set<int>> pos;
#define INF (1<<30)
int solve(string &s, string &t, int i = 0, int j = 0)
{
    if (j == t.size()) return 0;
    if (i == s.size()) return INF;
    if (dp[i][j] != -1) return dp[i][j];

    int res = INF;
    auto it = pos[t[j]].lower_bound(i);
    if (it != pos[t[j]].end())
        res = min(res, (*it - i + 1) + solve(s, t, *it+1, j+1));
    return dp[i][j] = res;
}
string minWindow(string &s, string &t)
{
    dp = vector<vector<int>>(s.size(), vector<int>(t.size(), -1));
    pos.clear();
    for (int i = 0; i < s.size(); ++i) pos[s[i]].insert(i);

    int res = INF, pos;
    for (int i = 0; i < s.size(); ++i)
    {
        int cur = solve(s, t, i);
        if (cur < res) res = cur, pos = i;
    }
    
    return (res == INF) ? "" : s.substr(pos, res);
}
// O(NK logN)
```

## Rope

* Insert into a string so APAP add XYZ so APXYZAP
* Delete from string
* Move pieces around

All these in O\(logN\)

'binary' string is inorder traversal of our tree

![](.gitbook/assets/image%20%28103%29%20%281%29.png)

Next important concept, is say we have to insert at pos 10, count subtree sizes so \(in right\) we will go to the right of x because 10 &gt; 8

![](.gitbook/assets/image%20%2838%29.png)

![](.gitbook/assets/image%20%2883%29%20%281%29.png)

```cpp
struct Rope
{
    Rope *left, *right, *parent;
    string str;
    int lCount;
};

// Maximum number of characters to be put in leaves
const int LEAF_LEN = 2;
void initRope(Rope *&node, Rope *par, string a, int l, int r)
{
    Rope *temp = new Rope();
    temp->left = temp->right = NULL;
    temp->parent = par;
    if ((r-l) > LEAF_LEN)
    {
        temp->str = "";
        temp->lCount = (r - l) / 2;
        node = temp;
        int m = l + (r - l)/2;
        initRope(node->left, node, a, l, m);
        initRope(node->right, node, a, m+1, r);
    }
    else
    {
        node = temp;
        temp->lCount = (r - l);
        stringstream ss;
        for (int i = l; i <= r; ++i) ss << a[i];
        temp->str = ss.str();
    }
}
void printString(Rope *rope)
{
    if (rope == NULL) return;
    if (rope->left == NULL && rope->right == NULL) cout << rope->str;
    printString(rope->left);
    printString(rope->right);
}
Rope* concatenate(Rope *&root1, Rope *root2, int n1)
{
    Rope *temp = new Rope();
    temp->parent = NULL;
    temp->left = root1, temp->right = root2;
    root1->parent = root2->parent = temp;
    temp->lCount = n1;
    temp->str = "";
    return temp;
}

int main()
{
    Rope *root1 = NULL;
    string s1 = "YO YO YO";
    initRope(root1, NULL, s1, 0, s1.size() - 1);
    Rope *root2 = NULL;
    string s2 = "NO NO";
    initRope(root2, NULL, s2, 0, s2.size() - 1);
    auto res = concatenate(root2, root1, 5);
    printString(res);
    return 0;
}
```

## String Hashing

We want to compare strings efficiently. The brute force way of doing so is just to compare the letters of both strings, which has a time complexity of O\(min\(n1,n2\)\) if n1 and n2 are the sizes of the two strings.

we convert each string into an integer, and compare those instead of the strings. Comparing two strings is then an O\(1\) operation. For comparison we use a hash function. The following condition has to hold: if two strings s and t are equal \(s=t\), then also their hashes have to be equal \(hash\(s\)=hash\(t\)\). Otherwise we will not be able to compare strings. Notice, the opposite direction doesn't have to hold. The reason why the opposite direction doesn't have to hold, if because there are exponential many strings. If we only want this hash function to distinguish between all strings consisting of lowercase characters of length smaller than 15, then already the hash wouldn't fit into a 64 bit integer.

So usually we want the hash function to map strings onto numbers of a fixed range \[0,m\), then comparing strings.

![Widely used here p \(prime\) and m are chosen, it&apos;s called polynomial hash](.gitbook/assets/image%20%2829%29.png)

For example, if the input is composed of only lowercase letters of English alphabet, p=31 is a good choice. Obviously m should be a large number, since the probability of two random strings colliding is about ≈1/m.

```cpp
long long compute_hash(string const& s)
{
    const int p = 31;
    const int m = 1e9 + 9;
    long long hash_value = 0;
    long long p_pow = 1;
    for (char c : s)
    {
        hash_value = (hash_value + (c - 'a' + 1) * p_pow) % m;
        p_pow = (p_pow * p) % m;
    }
    return hash_value;
}
```

### Search for duplicate strings in an array of strings

We calculate the hash for each string, sort the hashes together with the indices, and then group the indices by identical hashes.

```cpp
vector<vector<int>> group_identical_strings(vector<string> const& s)
{
    int n = s.size();
    vector<pair<long long, int>> hashes(n);
    for (int i = 0; i < n; i++)
        hashes[i] = {compute_hash(s[i]), i};

    sort(hashes.begin(), hashes.end());

    vector<vector<int>> groups;
    for (int i = 0; i < n; i++)
    {
        if (i == 0 || hashes[i].first != hashes[i-1].first)
            groups.emplace_back();
        groups.back().push_back(hashes[i].second);
    }
    return groups;
}
```

### Fast hash calculation of substring of given string

![](.gitbook/assets/image%20%2863%29%20%281%29.png)

### Number of different substrings in a string \(N^2 log N\)

```cpp
int count_unique_substrings(string const& s)
{
    int n = s.size();
    const int p = 31;
    const int m = 1e9 + 9;
    vector<long long> p_pow(n);
    p_pow[0] = 1;
    for (int i = 1; i < n; i++)
        p_pow[i] = (p_pow[i-1] * p) % m;

    vector<long long> h(n + 1, 0);
    for (int i = 0; i < n; i++)
        h[i+1] = (h[i] + (s[i] - 'a' + 1) * p_pow[i]) % m;

    int cnt = 0;
    for (int l = 1; l <= n; l++)
    {
        set<long long> hs;
        for (int i = 0; i <= n - l; i++)
        {
            long long cur_h = (h[i + l] + m - h[i]) % m;
            cur_h = (cur_h * p_pow[n-i-1]) % m;
            hs.insert(cur_h);
        }
        cnt += hs.size();
    }
    return cnt;
}
```

## Rolling Hashes and Bloom Filters

If we want to encode a string:

Say "abcad" =&gt; 0.26^4 + 1.26^4 + 2.26^4 + ...  
But there are problems in this way say for "aaa" "aa" it is same  
**Fixes**: encode length in hash \(messy\), don't use 0

At the end we can take modulo of it to avoid very big number

![](.gitbook/assets/image%20%28135%29.png)

![](.gitbook/assets/image%20%2868%29.png)

![](.gitbook/assets/image%20%28125%29.png)

![](.gitbook/assets/image%20%2845%29.png)

![](.gitbook/assets/image%20%28183%29.png)

### Estimating Collision:

Pick prime number for modulus \(Cycle length if we just take any number greater then 1 and just multiply it repeatedly the cycle length before you get back the number mod p is exactly p-1. It's through fermat's little theorem so it is more uniformly distributed i.e. a ^ p-1 mod p = 1\)

Assuming that strings are uniform our probability of collision will be n/p

$$
P_{collision} = \prod_{i=1}^{n}(1 - (i-1)/p)
$$

Tip: Use multiple primes -&gt; 1e9 + 7, 1e9 + 9, 1e9 + 23

Why we can getaway with calculating only handful of different hashes comes from Chinese remainder theorem. X = 1 \(mod p1\) X = 2 \(mod p2\)  
If gcd\(p1, p2\) = 1 X is unique mod LCM\(p1, p2\) = p1.p2  
So we can now use multiple primes will now become n/p1.p2... So collision chances will reduce

### Tsukuba Regional Problem - Anagram

{% embed url="http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1370" %}

Basically given two strings say - "anagram" and "grandmother" find longest substring which is anagram to both so 'nagr' 'gran' is the answer.

If we take all \(s1 size\)^2 substring compute it's hash and then do a sliding window test thing on s2 for all we can find the answer. However hashset are pretty slow that's the problem so it will give TLE thats where bloom filter comes.

> In bloom filters say we are using 3 hash functions and we use 1 string "cat" it's hash a, b, c from those 3 functions will set ath bth and cth bit of bloom filter bitset. Later we search say "mat" and we get hashes c d e now we can check if the bit was previously set say it was 1 0 0. We take and of all to find result.  
> Bloom filter gives sure that if it's no then string was not searched but if yes it may be wrong result as well chances are less though.
>
> To avoid failures in bloom filter  
> - Make bitset hude i.e. 1e9  
> - Add more hash functions  
> - One bitset per hash function

Hashset are expensive at times bloom filters are better to use in this case.

TODO:

{% embed url="https://www.acmicpc.net/problem/14214" %}

## Suffix Array

It is a lexicographical sorted array of all suffix of given string. To save space instead of actually storing string we can store index.  
s = "abaab"  
suffices = \["abaab", "baab", "aab", "ab", "b"\]  
after sorting = \["aab", "ab", "abaab", "b", "baab"\]  
Therefore suffix array will be = \[2, 3, 0, 4, 1\]

### [Effectively constructing suffix array](https://codeforces.com/edu/course/2/lesson/2)
![](.gitbook/assets/suffixarray.png)
- In the final suffix array, instead of storing entire string store index from which till end the suffix corresponds.
- Append $ at end of each suffix (because it's lexiographically the smallest so it won't change any suffix order) and make all string same len.
- We can also apply radix sort on above preparation it will be: O(n * (n + 26)) i.e O(n^2)
- Finally make strings of len 2^k
- Total time: O(nlog^2n) we can optimize it to nlogn aswell
![](.gitbook/assets/suffixarray2.png)

```c++
const int alphabet = 256;
int strLen;
vector<int> sortCyclicShifts(string const& s)
{
    int n = s.size();
    vector<int> p(n), c(n), cnt(max(alphabet, n), 0);
    for (int i = 0; i < n; i++) cnt[s[i]]++;
    for (int i = 1; i < alphabet; i++) cnt[i] += cnt[i-1];
    for (int i = 0; i < n; i++) p[--cnt[s[i]]] = i;
    c[p[0]] = 0;
    int classes = 1;
    for (int i = 1; i < n; i++)
    {
        if (s[p[i]] != s[p[i-1]]) classes++;
        c[p[i]] = classes - 1;
    }
    vector<int> pn(n), cn(n);
    for (int h = 0; (1 << h) < n; ++h)
    {
        for (int i = 0; i < n; i++)
        {
            pn[i] = p[i] - (1 << h);
            if (pn[i] < 0) pn[i] += n;
        }
        fill(cnt.begin(), cnt.begin() + classes, 0);
        for (int i = 0; i < n; i++) cnt[c[pn[i]]]++;
        for (int i = 1; i < classes; i++) cnt[i] += cnt[i-1];
        for (int i = n-1; i >= 0; i--) p[--cnt[c[pn[i]]]] = pn[i];
        cn[p[0]] = 0; classes = 1;
        for (int i = 1; i < n; i++)
        {
            pii cur = {c[p[i]], c[(p[i] + (1 << h)) % n]};
            pii prev = {c[p[i-1]], c[(p[i-1] + (1 << h)) % n]};
            if (cur != prev) ++classes;
            cn[p[i]] = classes - 1;
        }
        c.swap(cn);
    }
    return p;
}
vector<int> constructSuffixArray(string s)
{
    s += "$"; strLen = s.size();
    vector<int> sorted_shifts = sortCyclicShifts(s);
    sorted_shifts.erase(sorted_shifts.begin());
    return sorted_shifts;
}
```

### Substring Search
- Each substring is the prefix of some suffix
- Since our suffix array is already sorted apply binary search to it and find a suffix whose prefix include k
- Total time: logN * len(p) along with NlogN building
```c++
bool SearchSubstring(vector<int> &suffArr, string &s, string &k)    // s is original string, k is substr to find
{
    int l = 0, r = suffArr.size()-1;
    while (l <= r)
    {
        int mid = l + (r-l)/2, f = 0;
        for (int i = 0; i < k.size(); ++i)
        {
            if (s[suffArr[mid] + i] < k[i]) { f = -1; break; }
            if (s[suffArr[mid] + i] > k[i]) { f = 1; break; }
        }
        if (f == 0) return true;
        if (f < 0) l = mid+1;
        else r = mid-1;
    }
    return false;
}
```

### Count substrings
- Total time: logN * len(p) along with NlogN building
```c++
int lowerBoundSearchSubstring(vector<int> &suffArr, string &s, string &k)
{
    int l = 0, r = suffArr.size()-1, val = -1;
    while (l <= r)
    {
        int mid = l + (r-l)/2;
        string nw = s.substr(suffArr[mid], k.size());
        if (k == nw) val = mid, r = mid-1;
        else if (k < nw) r = mid-1;
        else l = mid+1;
    }
    return val;
}
int upperBoundSearchSubstring(vector<int> &suffArr, string &s, string &k)
{
    int l = 0, r = suffArr.size()-1, val = -1;
    while (l <= r)
    {
        int mid = l + (r-l)/2;
        string nw = s.substr(suffArr[mid], k.size());
        if (k == nw) val = mid+1, l = mid+1;
        else if (k < nw) r = mid-1;
        else l = mid+1;
    }
    return val;
}
int countSubstring(vector<int> &suffArr, string &s, string &k)
{
    return upperBoundSearchSubstring(suffArr, s, k) - lowerBoundSearchSubstring(suffArr, s, k);
}
```

### LCP (Longest Common Prefix) of two strings
![](.gitbook/assets/lcp.png)
```c++
vector<int> constructLCP(string const& s, vector<int> const& p)     // string s and it's suffix array p
{
    int n = s.size();
    vector<int> rank(n, 0);
    for (int i = 0; i < n; i++) rank[p[i]] = i;
    int k = 0;
    vector<int> lcp(n-1, 0);
    for (int i = 0; i < n; i++)
    {
        if (rank[i] == n - 1) { k = 0; continue; }
        int j = p[rank[i] + 1];
        while (i + k < n && j + k < n && s[i+k] == s[j+k]) k++;
        lcp[rank[i]] = k;
        if (k) k--;
    }
    return lcp;
}
```

### Count number of different substrings
```c++
int numberOfDiffSubstrings(vector<int> &lcp, string &s)
{
    int n = s.size();
    return ((n*(n+1))/2 - accumulate(lcp.begin(), lcp.end(), 0LL));
}
```

### Longest Common Substring
```c++
string s, t; cin >> s >> t;
string str = s + '#' + t;
auto sa = constructSuffixArray(str);
auto lcp = constructLCP(str, sa);
string res = "";
for (int i = 0; i < str.size()-1; ++i)
{
    if ((sa[i] < s.size()) ^ (sa[i+1] < s.size()))
    {
        if (lcp[i] > res.size())
            res = str.substr(sa[i], lcp[i]);
    }
}
cout << res << '\n';
```

## Suffix Tree / Trie

```cpp
// Pointer based implementation
struct node
{
    char data;
    unordered_map<char, node*> next;
    bool isTerminal;
    node(char d) { data = d, isTerminal = false; }
};
class Trie
{
    node* root;
public:
    Trie() { root = new node('\0'); }
    void addWord(string word)
    {
        node* temp = root;
        for (int i = 0; word[i] != '\0'; ++i)
        {
            char ch = word[i];
            if (temp -> next.count(ch) == 0)
            {
                node* child = new node(ch);
                temp -> next[ch] = child;
                temp = child;
            }
            else temp = temp -> next[ch];
        }
        temp->isTerminal = true;
    }
    bool search(string word)
    {
        node* temp = root;
        for (int i = 0; word[i] != '\0'; ++i)
        {
            char ch = word[i];
            if (temp -> next.count(ch)) temp = temp -> next[ch];
            else return false;
        }
        return temp -> isTerminal;
    }
};

// Array based implementation, faster because no dynamic allocation
const int MAXN = 1e5;
int trieArr[MAXN+1][26], nxt = 1;
bool isTerminal[MAXN+1];
void insert(const string &str)
{
    int node = 0;
    for (auto &ch : str)
    {
        if (trieArr[node][ch-'a'] == 0)
        {
            trieArr[node][ch-'a'] = nxt;
            node = nxt;
            nxt++;
        }
        else node = trieArr[node][ch-'a'];
    }
    isTerminal[node] = true;
}
bool find(const string &str)
{
    int ind = 0;
    for (auto &ch : str)
    {
        if (trieArr[ind][ch-'a'] == 0) return false;
        else ind = trieArr[ind][ch-'a'];
    }
    return isTerminal[ind];
}
```

### [Longest Common Prefix using Tries](https://leetcode.com/problems/longest-common-prefix)

```cpp
class Solution {
    struct node
    {
        char data;
        int cnt;
        unordered_map<char, node*> next;
        node(char d) { data = d, cnt = 0; }
    };
    class Trie
    {
        node* root;
    public:
        Trie() { root = new node('\0'); }
        void addWord(string word)
        {
            node* temp = root;
            temp->cnt++;
            for (int i = 0; word[i] != '\0'; ++i)
            {
                char ch = word[i];
                if (temp -> next.count(ch) == 0)
                {
                    node* child = new node(ch);
                    temp -> next[ch] = child;
                    temp = child;
                }
                else temp = temp -> next[ch];
                temp->cnt++;
            }
        }
        string lcp()
        {
            string res = "";
            if (!root) return res;
            node *temp = root;
            while (temp)
            {
                if (temp->next.size() == 1)
                {
                    char val = temp->next.begin()->first;
                    temp = temp->next.begin()->second;
                    if (temp && temp->cnt == root->cnt)
                        res += val;
                }
                else break;
            }
            return res;
        }
    };

public:
    string longestCommonPrefix(vector<string>& strs)
    {
        Trie trie;
        for (auto &x : strs) trie.addWord(x);
        return trie.lcp();
    }
};
```

## Pattern Matching

```cpp
// Brute force O(MN)
int strStr(string haystack, string needle)
{
    if (needle.size() == 0) return 0;
    for (int i = 0, j = 0; i+j < haystack.size(); ++i)
    {
        for (j = 0; j < needle.size(); ++j)
        {
            if (needle[j] != haystack[i+j]) break;
            if (j == needle.size()-1) return i;
        }
    }
    return -1;
}

// KMP Algorithm
// https://youtu.be/4jY57Ehc14Y
// lps[i] is length of longest proper prefix of str[0..i] which is also suffix of str[0..i]
int strStr(string haystack, string needle)
{
    if (needle.size() == 0) return 0;

    vector<int> lps(needle.size());
    lps[0] = 0;
    for (int i = 1, j = 0; i < needle.size(); )
    {
        if (needle[i] == needle[j]) lps[i++] = ++j;
        else if (j != 0) j = lps[j-1];      // Consider this example aaacaaaa
        else lps[i++] = 0;
    }

    for (int i = 0, j = 0; i < haystack.size(); )
    {
        if (needle[j] == haystack[i+j]) ++j;
        if (j == needle.size()) return i;
        else if (i < haystack.size() && needle[j] != haystack[i+j])
        {
            if (j == 0) ++i;
            else i+=(j-lps[j-1]), j=lps[j-1];
        }
    }
    return -1;
}
```

### Rabin Karp implementation using rolling hash

![](.gitbook/assets/image%20%2811%29.png)

Window is taken, at each iteration push last and pull first is called in rolling hash generated.  
Once hash matches with pattern window we can also check to be fully sure.

## Aho-Corasick Algorithm for Pattern Matching

Given an input text of length n and an array of k words, arr\[\], find all occurrences of all words in the input text. m is count of letters in all arr\[\] words. It is used in Unix fgrep command.

If we iterate over words and perform pattern matching complexity will be O\(nk + m\) with this algo it will be O\(n + m + z\) where z is total number of occurrences of word in the text.

## Z Algorithm

```cpp
// N^2 version
vector<int> zFuncTrivial(string s)
{
    int n = s.size();
    vector<int> z(n, 0);
    for (int i = 1; i < n; ++i)
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) ++z[i];
    return z;
}

vector<int> zFunc(string s)
{
    int n = s.size();
    vector<int> z(n, 0);
    for (int i = 1, l = 0, r = 0; i < n; ++i)
    {
        if (i <= r) z[i] = min(r-i+1, z[i-l]);
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) ++z[i];
        if (i + z[i] - 1 > r) l = i, r = i + z[i] - 1;
    }
    return z;
}
```

{% embed url="https://codeforces.com/contest/126/problem/B" %}

```cpp
/* Problem: given string find a longest substring such that
it appears at the front, end & inside.
eg: "fixprefixsuffix" -> "fix"

using hashmap with binary search takes O(nLogn) Z algorithm
takes O(N)

As we iterate over the letters in the string (index i from 1 to
n - 1), we maintain an interval [L, R] which is the interval with
maximum R such that 1 ≤ L ≤ i ≤ R and S[L...R] is a prefix-substring
(if no such interval exists, just let L = R =  - 1) */

string s;
cin >> s;
vector<int> z(s.size());
int n = s.size();
int L = 0, R = 0;
/* Z algorithm produces an array z such that z[i] is length
of largest substring which is also the prefix and starting
from i */
for (int i = 1; i < n; ++i)
{
    // If i > R, then there does not exist a prefix-substring
    // of S that starts before i and ends at or after i.
    if (i > R)
    {
        L = R = i;
        while (R < n && s[R - L] == s[R]) R++;
        z[i] = R - L;
        R--;
    }
    else
    {
        int k = i - L;
        if (z[k] < R - i + 1) z[i] = z[k];
        else
        {
            L = i;
            while (R < n && s[R - L] == s[R]) R++;
            z[i] = R - L;
            R--;
        }
    }
}
/* example:
   0 0 0 0 0 0 3 0 0 0 0 1 3 0 0
   f i x p r e f i x s u f f i x */
int maxz = 0, res = 0;
for (int i = 1; i < n; i++)
{
    if (z[i] == n-i && maxz >= n-i) { res = n-i; break; }
    maxz = max(maxz, z[i]);
}
/* example (for if condition):
   0 0 0 0 0 0 12 0 0 0 0 0 6 0 0 0 0 0
   q w e r t y  q w e r t y q w e r t y
   without if condition ans will get qwertyqwerty */
if (res == 0) cout << "Just a legend\n";
else cout << s.substr(0, res) << "\n";
```

Z Algorithm as pattern matching

{% embed url="https://youtu.be/CpZh4eF8QBw" %}

```text
Text = xabcabzabc
Pattern = abc
Patter$Text  ($ or any char that's not present in strings)
    abc$xabcabzabc
z   00000300200300
         *     *
take the index with z value = pattern size subtract index
by (pattern size + 1) that's the answer
O(M + N) Time
```

### Repeated String Match(https://leetcode.com/problems/repeated-string-match/)
```c++
// Naive way - O(a*b) and linear space
int repeatedStringMatch(string a, string b)
{
    for (int i = 0; i < a.size(); ++i)
    {
        if (a[i] == b[0])
        {
            int cnt = 1, j = 0, startInd = i;
            while (j < b.size() && a[startInd] == b[j])
            {
                ++j, ++startInd;
                if (startInd >= a.size() && j < b.size()) startInd %= a.size(), cnt++;
            }
            if (j == b.size()) return cnt;
        }
    }
    return -1;
}

// Laughingly this works way faster than above
int repeatedStringMatch(string a, string b)
{
    string tmp = a;
    int maxCnt = b.size()/a.size() + 2;
    for (int i = 1; i <= maxCnt; ++i)
    {
        if (a.find(b) != string::npos) return i;
        a += tmp;
    }
    return -1;
}

// KMP linear time
int repeatedStringMatch(string a, string b)
{
    if (b.size() == 0) return 0;
    
    vector<int> lps(b.size());
    lps[0] = 0;
    for (int i = 1, j = 0; i < b.size(); )
    {
        if (b[i] == b[j]) lps[i++] = ++j;
        else if (j != 0) j = lps[j-1];
        else lps[i++] = 0;
    }

    for (int i = 0, j = 0; i < a.size(); )
    {
        if (b[j] == a[(i+j)%a.size()]) ++j;
        if (j == b.size())
        {
            if ((i+j)%a.size()) return (i+j)/a.size()+1;
            return (i+j)/a.size();
        }
        else if (i < a.size() && b[j] != a[(i+j)%a.size()])
        {
            if (j == 0) ++i;
            else i+=(j-lps[j-1]), j=lps[j-1];
        }
    }

    return -1;
}
```

Other Problems:  
[https://www.codechef.com/problems/CHSTR](https://www.codechef.com/problems/CHSTR)  
[http://www.spoj.com/problems/SUFEQPRE/](http://www.spoj.com/problems/SUFEQPRE/)  
[https://codeforces.com/contest/119/problem/D](https://codeforces.com/contest/119/problem/D)

## Suffix Automata

![The above picture shows an automaton for the string &quot;abcbc&quot;](.gitbook/assets/image%20%2852%29.png)

![Longest &amp; Shortest string in equivalence class and suffix link \(in blue\)](.gitbook/assets/image%20%28158%29.png)

```cpp
// O(N)
struct SuffixAutomaton
{
    vector<map<char, int>> edges;
    vector<int> link, length;
    unordered_map<int, bool> terminals;
    int last;
    SuffixAutomaton(string s)
    {
        edges.push_back(map<char, int>());
        link.push_back(-1);
        length.push_back(0);
        last = 0;
        for (int i = 0; i < s.size(); i++)
        {
            edges.push_back(map<char, int>());
            length.push_back(i + 1);
            link.push_back(0);
            int r = edges.size() - 1;
            int p = last;
            while (p >= 0 && edges[p].find(s[i]) == edges[p].end()) edges[p][s[i]] = r, p = link[p];
            if (p != -1)
            {
                int q = edges[p][s[i]];
                if (length[p] + 1 == length[q]) link[r] = q;
                else
                {
                    edges.push_back(edges[q]);
                    length.push_back(length[p] + 1);
                    link.push_back(link[q]);
                    int qq = edges.size() - 1;
                    link[q] = qq;
                    link[r] = qq;
                    while (p >= 0 && edges[p][s[i]] == q) edges[p][s[i]] = qq, p = link[p];
                }
            }
            last = r;
        }
        int p = last;
        while (p) terminals[p] = 1, p = link[p];
    }
};
/*
edges:   0    1    2    3    4    5    6    7
        a=1  b=2  c=3  b=4  c=6  c=7   -   b=4
        b=5
        c=7
last: 6
length: 0 1 2 3 4 1 5 2
link:  -1 0 5 7 5 0 7 0
terminals: 6 7
*/
```

![](.gitbook/assets/image%20%28163%29.png)

Applications:

```cpp
// Pattern Matching
SuffixAutomaton sa(str);
bool res = true;
int n = 0;
for (char ch : str)
{
    if (sa.edges[n].find(ch) == sa.edges[n].end())
    {
        res = false;
        break;
    }
    n = a.edges[n][ch];
}
return res;

// Count Unique Substrings
int countUniqueSubstrings(SuffixAutomaton &sa, int cur = 0)
{
    int val = 0;
    for (auto x : sa.edges[cur]) val += countUniqueSubstrings(sa, x.second);
    return (cur == 0) ? val : val + 1;
}

// Lexiographically Kth Substring
// Given string find all substring and sort them then find kth
// Problem: https://codeforces.com/problemset/problem/128/B
void DFS(SuffixAutomaton &sa, vector<int> &occurences, vector<int> &words, int cur = 0)
{
    int occ = 0, wrd = 0;
    if (occurences[cur] != 0) return;
    if (sa.terminals[cur]) occ++, wrd++;
    for (auto x : sa.edges[cur])
    {
        DFS(sa, occurences, words, x.second);
        occ += occurences[x.second], wrd += words[x.second] + occurences[x.second];
    }
    occurences[cur] = occ, words[cur] = wrd;
}

string KthLexiographicalSubstring(SuffixAutomaton &sa, int k)
{
    vector<int> words(sa.edges.size()), occurences(sa.edges.size());
    DFS(sa, occurences, words);

    int node = 0;
    string res;
    while (k > 0)
    {
        int acc = 0;
        for (auto x : sa.edges[node])
        {
            int tmp = acc;
            acc += words[x.second];
            if (acc >= k)
            {
                node = x.second;
                k -= tmp + occurences[x.second];
                res += x.first;
                break;
            }
        }
    }
    return res;
}
```



## Palindromic Tree

Palindromic Tree is based on Manacher's Algorithm, but it's easier to understand and can be extended to other applications as well. This data structure is infact very new.

Every node is a palindrome, edge u-&gt;v represents we can get v by adding that character both end to u. Next there are suffix link \(dotted\) u-&gt;v because \(like here\) a is largest suffix palindrome of aba.

![](.gitbook/assets/image%20%28152%29.png)

```cpp
struct PalindromicTree
{
    int MAXN, len, num, suff;
    struct node { int len, sufflink, num, next[26]; };
    node *tree;
    long long ans;
    char *s;

    PalindromicTree(string &str)
    {
        MAXN = str.size() + 5;
        tree = new node[MAXN];
        s = new char[MAXN];
        strcpy(s, str.c_str());
        num = 2, suff = 2;
        tree[1].len = -1, tree[1].sufflink = 1, tree[2].len = 0, tree[2].sufflink = 1;
    }
    ~PalindromicTree() { delete[] tree, delete[] s; }
    bool addLetter(int pos)
    {
        int cur = suff, curlen = 0, let = s[pos] - 'a';
        while (true)
        {
            curlen = tree[cur].len;
            if (pos - 1 - curlen >= 0 && s[pos - 1 - curlen] == s[pos]) break;
            cur = tree[cur].sufflink;
        }
        if (tree[cur].next[let]) { suff = tree[cur].next[let]; return false; }
        num++, suff = num, tree[num].len = tree[cur].len + 2, tree[cur].next[let] = num;
        if (tree[num].len == 1)
        {
            tree[num].sufflink = 2, tree[num].num = 1;
            return true;
        }
        while (true)
        {
            cur = tree[cur].sufflink, curlen = tree[cur].len;
            if (pos - 1 - curlen >= 0 && s[pos - 1 - curlen] == s[pos])
            {
                tree[num].sufflink = tree[cur].next[let];
                break;
            }
        }
        tree[num].num = 1 + tree[tree[num].sufflink].num;
        return true;
    }
};
```

Applications:

```cpp
// 1) Count number of palindromic substrings in O(N)
// https://informatics.mccme.ru/moodle/mod/statements/view.php?chapterid=1750
string str;
cin >> str;
PalindromicTree pt(str);
int ans = 0;
for (int i = 0; i < str.size(); ++i)
{
    pt.addLetter(i);
    ans += pt.tree[pt.suff].num;
}
cout << ans << endl;

/* 2) Shady String (Last question of CodeAgon 2019
    A shady string is a palindromic string of odd length.
    Em has a string s and he is looking for such a pair of
    shady strings that are non-overlapping and exists in
    string S. he also wants to find such a shady string
    pair whose product of lengths give the maximum possible
    value.
    
    Input:
    3
    5 5
    82 79 38 49 9
    5 9
    47 37 75 93 89
    5 2
    120 160 176 172 192
    
    Output:
    44
    56
    18
    
    Explanation:
    - For the first testcase, it s impossible to divide any
      number by 5, and the most optimal solution would be to
      multiply last number by 5 to get: [82, 79, 38, 49, 45]
    - For the second testcase, not changing the array gives the
      optimal answer.
    - The most optimal array after performing a sequence of
      operations would be: [30, 40, 44, 43, 48] */
```

