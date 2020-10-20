# Number Theory

## Primes and factors

* For every number n &gt; 1, there is a unique prime factorization

![](.gitbook/assets/image%20%2866%29.png)

* Number of factors of a number n is:

![There are &#x3B1;i + 1 ways to choose how many times prime pi appears in the factor](.gitbook/assets/image%20%282%29.png)

* Sum of factors of n is:

![](.gitbook/assets/image%20%2878%29%20%281%29.png)

* Product of factors of n is:

![we can form &#x3C4;\(n\)/2 pairs from the factors, each with product n](.gitbook/assets/image%20%2816%29.png)

* Goldbach’s conjecture: Each num &gt; 2 can be represented as sum of 2 primes [https://codeforces.com/contest/1238/problem/A](https://codeforces.com/contest/1238/problem/A)
* Twin prime conjecture: There is an infinite number of pairs \(p, p+2\) where both are primes.
* Legendre's conjecture: There's always a prime between n^2 and \(n+1\)^2

### Find Divisors

```cpp
vector<ll> findDivisors(ll n)
{
    vector<ll> res;
    for (ll i = 1; i <= sqrt(n); ++i)
    {
        if (n%i == 0)
        {
            res.push_back(i);
            if (n/i != i) res.push_back(n/i);
        }
    }
    return res;
}
```

### Sieve of Eratosthenes - O(nloglogn)

```cpp
const int MAXN = 1e5;
ll sieve[MAXN+1];
vector<int> primes;
void makeSieve()
{
    fill(sieve, sieve+MAXN+1, 1);
    sieve[0] = 0, sieve[1] = 0;
    for (int i = 2; i <= MAXN; ++i)
    {
        if (sieve[i] != 1) continue;
        for (int j = i*2; j <= MAXN; j += i) sieve[j] = 0;
    }
    for (int i = 0; i <= MAXN; ++i)
        if (sieve[i]) primes.push_back(i);
}

// Almost twice fast sieve can generate upto 3e7 range in 1 second
const int MAXN = 1e7;
int sieve[MAXN+1], primes[MAXN+1], primesSz = 0;
void findprimes()
{
    for (int i = 2; i <= MAXN; ++i)
    {
        if (sieve[i] == 0) { sieve[i] = i; primes[primesSz++] = i; }
        for (int j = 0, x = i*primes[j]; j < primesSz && primes[j] <= sieve[i]
            && x <= MAXN; ++j, x = i*primes[j])
            sieve[x] = primes[j];
    }
}

/*
- generate price prime upto sqrt[r]
- create an array of size r-l+1 and set all element to be 0 (0 means prime, 1 means composite)
- For each prime p in range 2 to sqrt[r]: for every multiple of p in range l to r, mark m-l as 1

l = 11, r = 20
[11 12 13 14 15 16 17 18 19 20]
For prime 2, mark 12, 14, 16, 18, 20 as composite
*/
int segmentedSieve(int n)
{
    vector<int> primes;
    int nsqrt = sqrt(n);
    vector<char> is_prime(nsqrt + 1, true);
    for (int i = 2; i <= nsqrt; i++)
    {
        if (is_prime[i])
        {
            primes.push_back(i);
            for (int j = i * i; j <= nsqrt; j += i)
                is_prime[j] = false;
        }
    }

    const int S = 10000;
    int result = 0;
    vector<char> block(S);
    for (int k = 0; k * S <= n; k++)
    {
        fill(block.begin(), block.end(), true);
        int start = k * S;
        for (int p : primes)
        {
            int start_idx = (start + p - 1) / p;
            int j = max(start_idx, p) * p - start;
            for (; j < S; j += p)
                block[j] = false;
        }
        if (k == 0)
            block[0] = block[1] = false;
        for (int i = 0; i < S && start + i <= n; i++)
        {
            if (block[i])
                result++;
        }
    }
    return result;
}

```

### Euler totient function

Counts the number of integers between 1 and n inclusive, which are coprime to n
```cpp
// Euler totient function in logN
int phi(int n)
{
    int res = n;
    for (int i = 2; i*i <= n; ++i)
    {
        if (n%i == 0)
        {
            while (n%i == 0) n /= i;
            res -= res/i;
        }
    }
    if (n > 1) res -= res/n;
    return res;
}

// Euler totient preprocessing
const int MAXN = 1e5;
int phi[MAXN+1];
void phiPreprocess()
{
    phi[0] = 0, phi[1] = 1;
    for (int i = 2; i <= MAXN; ++i) phi[i] = i-1;
    for (int i = 2; i <= MAXN; ++i)
        for (int j = 2*i; j <= MAXN; j += i) phi[j] -= phi[i];
}
```

![](.gitbook/assets/image%20%28149%29.png)

### Eucledian Algorithm

![If negative number take abs and put it will work just fine. LCM\(a, b\) \* gcd\(a, b\) = a\*b](.gitbook/assets/image%20%28153%29.png)

```cpp
inline int gcd (int a, int b) { while (b) { a %= b; swap(a, b); } return a; }
inline int lcm (int a, int b) { return a / gcd(a, b) * b; }
```

**Correctness Proof:**  
[**https://youtu.be/H\_2\_nqKAZ5w**](https://youtu.be/H_2_nqKAZ5w)  
****For proof we need to show gcd\(a, b\) = gcd\(b, a%b\) for all a &gt;= 0 & b &gt;= 0. One thing to notice is second argument is strictly decreasing  
Let d = gcd\(a, b\) then by definition d\|a and d\|b \(d divides a and d divides b\)  
We know a%b = a - b\*floor\(a/b\)  
From this we follow that gcd\(b, a%b\) by definition d\|b and d\|\(a%b\)  
Say there are 3 integers p, q, r - if p\|q and p\|r then p\|gcd\(q, r\) using this we can get gcd\(a, b\) \| gcd\(b, a%b\)  
Thus we have shown left side of the original solution divides right side. The second half of proof is similar.

**Time Complexity:**  
Runtime is estimated by Lame's Theorem which establishes connected with fibonacci sequence \(formed by successive a%b\). Making it O\(log min\(a, b\)\)

## Modular Arithmetic

![](.gitbook/assets/image%20%2826%29.png)

### Modular Exponentiation

![](.gitbook/assets/image%20%28180%29.png)

```cpp
int powMod(int a, int b)
{
    int x = 1;
    while (b > 0)
    {
        if (b & 1) x = (x * a) % MOD;
        a = (a * a) % MOD;
        b >>= 1;
    }
}
```

![](.gitbook/assets/image%20%2891%29.png)

* [Leading and Trailing](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1970): `pow(10, fmod(klog10(n), 1))`\*`100;` and `powMod(n, k, 1000);`
* [Locker](http://www.spoj.com/problems/LOCKER/): Simply we want a seq. with max product and sum = n, OEIS [https://oeis.org/A000792](https://oeis.org/A000792)
* [Just add it](http://www.spoj.com/problems/ZSUM/): [https://ideone.com/6E5bc7](https://ideone.com/6E5bc7)
* [Jewel-eating Monsters](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1723): Final solution becomes: `((x-1) * a^n) - (a(a^n-1 -1))/(a-1)`
* [Parking Lot](http://codeforces.com/problemset/problem/630/I): Consider 3 types of placements: XXXOOOO, OOOOXXX, OOXXXOO i.e. left right or anywhere middle first two conditions are handled by: 2.4.3.4^\(n-3\) middle one is handled by \(n-3\).4.3^2.4^\(n-4\).

### Fermat's Theorem and Euler Theorem

![](.gitbook/assets/image%20%2836%29.png)

### Modular Inverse

![](.gitbook/assets/image%20%2832%29.png)

### nCr

```cpp
int nCr(int n, int r)
{
    if (n < r) return 0;
    ll res = 1, rem = 1;
    for (ll i = n - r + 1; i <= n; i++) res = multiply(res, i);
    for (ll i = 2; i <= r; i++) rem = multiply(rem, i);
    return multiply(res, powMod(rem, MOD - 2));
}
const int MAXN= 1e5;
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
```

**Properties:**

![](.gitbook/assets/image%20%28172%29.png)

![](.gitbook/assets/image%20%2837%29.png)

![](.gitbook/assets/image%20%28126%29.png)

## Fibonacci Number

* **Cassini's Identity:** `F(n-1)F(n+1) - F(n)F(n) = (-1)^n`
* **Addition Rule:** `F(n+k) = F(k)F(n+1) + F(k-1)F(n)`
* **Applying previous identity where k = n:** `F(2n) = F(n) (F(n+1) + F(n-1))`
* **From this we can prove by induction that F\(nk\) is multiple of F\(n\)**
* **GCD Identity:** gcd\(F\(m\), F\(n\)\) = F\(gcd\(m, n\)\)

### Fibonacci Coding

According to Zeckendorf's theorem, any natural number n can be uniquely represented as a sum of Fibonacci numbers.

![](.gitbook/assets/image%20%2821%29.png)

Idea is to represent in binary notation with end as 1 \(to mark ending\) for di = 1 means F\(i+2\) is taken. It can be filled by greedily picking maximum size fibonacci and subtracting.

### Formulas

![](.gitbook/assets/image%20%28141%29.png)

![](.gitbook/assets/image%20%28128%29.png)





## Solving Equations

A diophatine equation is of the for `ax + by = c`  where a, b, c are constants and value of x & y have to be found. We want to:

> If a = b = 0 then either we have no solution or infinite solutions

```cpp
// Not to be used directly
pii chineaseRemainderTheorem(int x, int a, int y, int b)
{
    int s, t, d = extendedEuclid(x, y, s, t);
    if (a%d != b%d) return make_pair(0, -1);
    return make_pair(((s*b*x + t*a*y + x*y)%(x*y))/d, x*y/d);
}

// returns d = gcd(a,b); finds x,y such that d = ax + by
int extendedEuclid(int a, int b, int &x, int &y)
{
    int xx = y = 0, yy = x = 1;
    while (b)
    {
        int q = a/b;
        int t = b; b = a%b; a = t;
        t = xx; xx = x-q*xx; x = t;
        t = yy; yy = y-q*yy; y = t;
    }
    return a;
}
// finds all solutions to ax = b (mod n)
vector<int> modularLinearEquationSolver(int a, int b, int n)
{
    int x, y;
    vector<int> solutions;
    int d = extendedEuclid(a, n, x, y);
    if (b%d == 0)
    {
        x = ((x * (b/d) + n) % n);
        for (int i = 0; i < d; ++i)
            solutions.push_back((x + i*(n/d) + n) % n);
    }
    return solutions;
}
// computes x and y such that ax + by = c; on failure, x = y =-1
void linearDiophantine(int a, int b, int c, int &x, int &y)
{
    int d = __gcd(a, b);
    if (c%d) x = y = -1;
    else
    {
        x = c/d * inverseMod(a/d, b/d);
        y = (c-a*x)/b;
    }
}
// finds z such that z % x[i] = a[i] for all i. returns (z, M) on failure M = -1
// Note: we require a[i]'s to be relatively prime
pii chineaseRemainderTheorem(const vector<int> &x, const vector<int> &a)
{
    pii ret = make_pair(x[0], a[0]);
    for (int i = 1; i < x.size(); ++i)
    {
        ret = chineaseRemainderTheorem(ret.first, ret.second, x[i], a[i]);
        if (ret.second == -1) break;
    }
    return ret;
}
```

## Other Results

### Lagrange's Four Square theorem

Every natural number can be represented as the sum of four integer squares

![](.gitbook/assets/image%20%286%29.png)

Example problem: [https://leetcode.com/problems/perfect-squares/](https://leetcode.com/problems/perfect-squares/)

> Minimum number of squares required is 4 if n can be written in the form 4^k\(8m + 7\)

```cpp
// No DP No BFS this method is the best XD
int numSquares(int n)
{
    int srt = sqrt(n);
    if (is_square(n)) return 1;

    for (int i = 1; i <= sqrt(n); ++i)
        if (is_square(n - i*i)) return 2;

    while (n%4 == 0) n >>= 2;
    if (n % 8 == 7) return 4;
    
    return 3;
}
```

### Pythagorean triples

It's a triple \(a, b, c\) that satisfies Pythagorean theorem a^2 + b^2 = c^2.  
If \(a, b, c\) is a valid triple all \(ka, kb, kc\) is also valid where k &gt; 1

**Euclid formula:**  `(n^2 - m^2, 2nm, n^2 + m^2)  
Where 0<n<m and n, m are coprime and atleast one of n, m is even. Example m=1 n=2 gives smallest triplet (3, 4, 5)`

## Matrix

### Degenerate Matrix

Matrix with determinant = 0, hence they are linear dependent and can be written as:

![](.gitbook/assets/image%20%287%29.png)

## Combinatorics

### Boxes and Balls

Count number of ways to place k balls in n boxes

* **Each box can contain at most 1 ball:** Ans is directly C\(n, k\)
* **Each box can contain multiple balls:** Consider it like a string o means put ball in that box - means move to next box. Using this our final string will contain k \(o's\) and n-1 \(-'s\) so ans is C\(k+n-1, k\)
* **Each box contain at most 1 ball and no 2 adjacent box contain ball:** We can assume there are k boxes initially to which adjacent boxes are inserted. There are n-2k+1 such boxes and k+1 positions for them using prev formula C\(n-k+1, n-2k+1\)

### Catalan Number

> 1,1,2,5,14,42,132, 429...

* Number of valid parenthesis expressions that consist of n left parentheses and n right parentheses.
* There are Cn binary trees of n nodes and Cn-1 rooted trees of n nodes.

![](.gitbook/assets/image%20%28115%29.png)

![](.gitbook/assets/image%20%28156%29.png)

![](.gitbook/assets/image%20%28202%29.png)

```cpp
unordered_map<int, int> dp;
int cat(int n)    // 1 indexed
{
    if (n <= 1) return 1;
    if (dp[n] != 0) return dp[n];
    int res = 0;
    for (int i = 0; i < n; ++i)
        res += cat(i) * cat(n-i-1);
    return dp[n] = res;
}
```

### Stirling Number of Second Kind

Given n numbers, grouping them in k subgroups find total possibility. It's defined by stirling number:  
S\(n, k\) = k\*\(S\(n-1, k\)\) + S\(n-1, k-1\)

### Inclusion Exclusion

\| A ∪ B ∪ C \| = \| A \| + \| B \| + \| C \| - \| A ∩ B \| - \| B ∩ C \| - \| C ∩ A \| + \| A ∩ B ∩ C\|  
 \| A ∪ B ∪ C ∪ ... \| = { SINGLE SUMS } - { DOUBLE PAIRS SUM } + { TRIPLE PAIRS SUM } - { FOUR PAIRS SUM } + ...

Find numbers less than 1000 divisible by 2, 3 & 5  
 Numbers less then N divisible by m are floor\(\(N - 1\) / m\) So:  
 Divisible by 2 = 449  
 Divisible by 3 = 333  
 Divisible by 5 = 199  
 Divisible by 2.5 = 99  
 Divisible by 3.5 = 66  
 Divisible by 2.3 = 166  
 Divisible by 2.3.5 = 33  
 \| 2 ∪ 3 ∪ 5 \| = 499 + 133 + 199 - 99 - 66 - 166 + 33 = 733

Numbers between 1 and n which are divisible by any of the prime numbers less than 20

```cpp
ll t;
cin >> t;
while (t--)
{
    ll num;
    cin >> num;
    ll arr[] = { 2, 3, 5, 7, 11, 13, 17, 19 };
    ll n = sizeof(arr) / sizeof(ll);
    ll result = 0;
    for (ll i = 1; i < (1<<n); ++i)
    {
        ll mask = i, temp = 1, pos = 0, product = 1ll, bits = __builtin_popcount(mask);
        while (mask > 0)
        {
            ll lastBit = (mask&1);
            if (lastBit) product *= arr[pos];
            mask >>= 1;
            ++pos;
        }
        if (bits&1) result += num/product;
        else result -= num/product;
    }
    cout << result << endl;
}
```

### Derangements

Number of permutations where no element remain in original place. It follows following relation:

![](.gitbook/assets/image%20%2849%29%20%281%29.png)

* There are \(n-1\) ways to chose an element x that can replace element 1.
* We swapped x and 1 element so now we are left with f\(n-2\)
* We replace element x with some other element \(than 1\) Now we have to construct n-1 derangements since we cannot replace x with \(1 val\)

### Burnside's Lemma

TODO

### Prüfer code

Way of encoding a labelled tree into a sequence of n-2 integers in interval \[0, n-1\]  
Cayley's formula states that the **number of spanning trees in a complete labeled graph** with n vertices is n^\(n-2\) 

Process: Removes n−2 leaves from the tree. At each step, the leaf with the smallest label is removed, and the label of its only neighbor is added to the code

![](.gitbook/assets/image%20%2858%29.png)

![Cayley&apos;s Formula](.gitbook/assets/image%20%28118%29.png)

```cpp
const int MAXN = 1e5;
vector<int> adj[MAXN+1];
int parent[MAXN+1];
void DFS(int u)
{
    for (auto &v : adj[u])
        if (v != parent[u]) { parent[v] = u; DFS(v); }
}
vector<int> prueferCode(int n)
{
    parent[n-1] = -1;
    DFS(n-1);
    int ptr = -1;
    vector<int> degree(n);
    for (int i = 0; i < n; i++)
    {
        degree[i] = adj[i].size();
        if (degree[i] == 1 && ptr == -1) ptr = i;
    }
    vector<int> code(n - 2);
    int leaf = ptr;
    for (int i = 0; i < n - 2; i++)
    {
        int next = parent[leaf];
        code[i] = next;
        if (--degree[next] == 1 && next < ptr) leaf = next;
        else { ptr++; while (degree[ptr] != 1) ptr++; leaf = ptr; }
    }
    return code;
}
vector<pii> prueferDecode(vector<int> &code)
{
    int n = code.size() + 2;
    vector<int> degree(n, 1);
    for (int i : code) degree[i]++;
    int ptr = 0;
    while (degree[ptr] != 1) ptr++;
    int leaf = ptr;
    vector<pii> edges;
    for (int v : code)
    {
        edges.emplace_back(leaf, v);
        if (--degree[v] == 1 && v < ptr) leaf = v;
        else { ptr++; while (degree[ptr] != 1) ptr++; leaf = ptr; }
    }
    edges.emplace_back(leaf, n-1);
    return edges;
}
```

[https://codeforces.com/contest/156/problem/D](https://codeforces.com/contest/156/problem/D)

## Probability

**Bernoulli Trial:** A dice thrown n times and we want probability for 6 exactly x times. so: nCx \(1/6\)^x \(5/6\)^\(n - x\)  
 The expected number of trials for ith success is 1/p

> **Coupon Collector Problem: A certain brand of cereal always distributes a coupon in every cereal box. The coupon chosen for each box is chosen randomly from a set of n distinct coupons. A coupon collector wishes to collect all n distinct coupons. What is the expected number of cereal boxes must the coupon collector buy so that the coupon collector collects all n distinct coupons.**  
>   
>  Probability of collecting first coupon is 1 since the collector has none. Later on for Pi = \(n - \(i-1\)\) / n  
>  E\(x\) = 1/P \(follows geometric distribution\) we need to calculate summation of E\[x\]  
>  = E\(1\) + E\(2\) + E\(3\) + E\(4\) + ... + E\(n\)  
>  = n/n + n/n-1 + n/n-2 + n/n-3 + ... + + n/2 + n/1  
>  = n \(1/n + 1/n-1 + 1/n-2 + 1/n-3 + ... + 1/2 + 1\)  
>  Apply expansion

### Pigeonhole Principle

Divisible Subset Problem \(C4\)\([https://www.codechef.com/problems/DIVSUBS](https://www.codechef.com/problems/DIVSUBS)\)  
 Example: 3 4 3 5 2 3  
 Naive approach is by exponential time finding pairs with 1 element only then 2 element only and so on.  
 Any such problem can be illustrated as \(1+x^3\)\(1+x^4\)\(1+x^3\)\(1+x^5\)\(1+x^2\)\(1+x^3\) solving this will give terms having powers of all subsets we just need to apply % N = 0.  
 It can be solved in O\(NlogN\) by Fast Fourier Transform or simply in O\(N2\) [https://www.youtube.com/watch?v=QQQpOa3aXew](https://www.youtube.com/watch?v=QQQpOa3aXew)

```text
Itterate all array elements and apply % N and record it in the array of vector below.
0   1   2   3   4   5
            0
Then 4
0   1   2   3   4   5
                1
Then in 4 to 3 so 4 + 3 % N
0   1   2   3   4   5
    0,1     0   1
and so on.

a[]     =      3    4    3    5    2    3
b[]     = 0    3    7    10   15   17   20
b[]     = 0    3    1    4    3    5    2
temp[]  =      2    6    1,4  3    5  
This is pigeonhole senerio in every case any temp arr will have two element.
end - start i.e. 4 - 1 = 3 so 3 elements that are 2nd 3rd and 4th (4 + 3 + 5) % 6 = 0
```

* P\(x & y\) = P\(x/y\) P\(y\)
* P\(x/y\) = P\(y/x\) P\(x\)/P\(y\)     - Baye's Theorem
* P\(x \| y\) = P\(x\) + P\(y\) - P\(x & y\)
* Independence rule: if x & y are independent events then P\(x & y\) = P\(x\) P\(y\)
* Mutual Exclusive: \(One happens then other cannot\) P\(x \| y\) = P\(x\) + P\(y\)

