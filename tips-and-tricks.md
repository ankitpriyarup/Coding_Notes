# Tips & Tricks

## VS Code Snippet Template

{% file src=".gitbook/assets/cpp.json" caption="cpp.json" %}

## Compilation flag

```bash
time g++ -std=c++17 cprog.cpp && timeout 5 ./a.out < i > o
```

With sanitizers

```bash
time g++ -std=c++17 cprog.cpp -Wshadow -Wall -g -fsanitize=address -fsanitize=undefined -D_GLIBCXX_DEBUG && timeout 5 ./a.out < i > o
```

## Stress Test

```cpp
#include <bits/stdc++.h>
using namespace std;

int LIMIT = 1000;
void generateTestCase()
{
    ofstream fout("i");
    int n = 20;
    fout << n << '\n';
    while (n--)
    {
        int x = 1 + rand()%1000;
        fout << x << " ";
    }
    fout << '\n';
    fout.close();
}
string inp, wa, ac;
int main()
{
    system("g++ -std=c++17 cprog.cpp -o ac.out");
    system("g++ -std=c++17 ctest.cpp -o wa.out");
    srand(time(0));
    while (LIMIT--)
    {
        generateTestCase();
        ifstream inpf("i");
        getline(inpf, inp, char(EOF));
        inpf.close();

        system("./wa.out < i > o");
        ifstream waf("o");
        getline(waf, wa, char(EOF));
        waf.close();

        system("./ac.out < i > o");
        ifstream acf("o");
        getline(acf, ac, char(EOF));
        acf.close();

        if (wa != ac)
        {
            cout << "Failed at:\n";
            cout << inp << '\n';
            cout << "Expected:\n";
            cout << ac << '\n';
            cout << "Found:\n";
            cout << wa << '\n';
            cout << "-----------\n\n";
        }
    }
    return 0;
}
```

### **Regex**

```cpp
string str = "Hello\tWorld\n";
string raw_str = R"(Hello\tWorld
this is ankit\n)";
/* Output:
Hello	World
Hello\tWorld
this is ankit\n

raw strings escape \t \n and considers real
new line instrings doing new line gives error
unless done with escape character \ like in
macro */

// Regex match
// https://youtu.be/sa-TUpSx1JA
/*
    .       - Any Character Except New Line
    \d      - Digit (0-9)
    \D      - Not a Digit (0-9)
    \w      - Word Character (a-z, A-Z, 0-9, _)
    \W      - Not a Word Character
    \s      - Whitespace (space, tab, newline)
    \S      - Not Whitespace (space, tab, newline)
    
    \b      - Word Boundary
    \B      - Not a Word Boundary
    ^       - Beginning of a String
    $       - End of a String
    
    []      - Matches Characters in brackets
    [^ ]    - Matches Characters NOT in brackets
    |       - Either Or
    ( )     - Group
    
    Quantifiers:
    *       - 0 or More
    +       - 1 or More
    ?       - 0 or One
    {3}     - Exact Number
    {3,4}   - Range of Numbers (Minimum, Maximum)
*/
regex email_pattern(R"(^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$)");
if (regex_match("ankitpriyarup@gmail.com", email_pattern))
    cout << "Yes" << endl;
else
    cout << "No" << endl;
```

### All, Any, None

```cpp
if (all_of(all(arr), [](const int x){ return x >= 1 && x <= 5; }))
    cout << "All values in range";

// simmlarly there are none_of any_of
```

### Policy Based Data Structures

Gives access to the indices that the elements would have in a sorted array.

```cpp
pbds A; // works in logN
A.insert(1); A.insert(3); A.insert(5); A.insert(7); A.insert(9); A.insert(11);

// Iterator of element at specified pos k
A.find_by_order(k)
// Position of x in sorted array
A.order_of_key(x)

// If the element is not present we get the pos that the element
// would have in set if present
```

### Matrix Exponentiation

```cpp
// Fibonacci
Matrix<int> T(2, 2), F(2, 1);
T.mat = {{0, 1}, {1, 1}};
F.mat = {{0}, {1}};
T ^= n;
T *= F;
cout << T.mat[0][0] << '\n';
```

### Unordered map with custom hashfunction
```c++
struct hash_pair
{ 
    template <class T1, class T2>
    size_t operator()(const pair<T1, T2>& p) const
    {
        auto hash1 = hash<T1>{}(p.first);
        auto hash2 = hash<T2>{}(p.second);
        return hash1 ^ hash2;
    }
};

unordered_map<pair<int, int>, int, hash_pair> rec;
```

### Lowerbound on set<pair<int, int>>
```c++
x.lower_bound({first, -inf});
```