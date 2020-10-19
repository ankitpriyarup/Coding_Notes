# Bit Manipulation

* C++ type int is a 32-bit type, which means that every int number consists of 32 bits
* Representation is indexed from right to left
* The bit representation of a number is either signed or unsigned
* **Least Significant Bit \(LSB\) & Most Significant Bit \(MSB\):** 99 in binary \(MSB part\)01100011\(LSB part\) so in MSB 01100011 in LSB 11000110 Endiness \(Storing data in memory\) : Little Endian \(LSB\), Big Endian \(MSB\)
* 1's Complement: Toggling every bit ~
* 2's Complement: -X = ~X + 1 X = ~\(-X-1\)
* \_\_builtin\_clz\(x\): the number of zeros at the beginning of the number \(count leading zeroes\)
* \_\_builtin\_ctz\(x\): the number of zeros at the end of the number \(count trailing zeroes\)
* \_\_builtin\_popcount\(x\): the number of ones in the number
* \(A+B\) = \(A^B\) + 2\(A&B\)
> Above equation is full adder, A^B is ans and A&B is carry. Final ans is A^B + 2*(A&B) multiplying by 2 in order to shift bit by one

```cpp
// Multiplication
int mul(int a, int b)
{
    int ans = 0;
    for (int i = 0 ; i < 32; ++i)
    {
        if (b & 1) ans += a;
        b = b>>1;
        a = a<<1;
    }
    return ans;
}

// Swap two numbers without extra space
int x = 10, y = 5;
x = x ^ y;
y = x ^ y;      //y = (x ^ y) ^ y = (y ^ y) ^ x = 0 ^ x = x
x = x ^ y;      //x = (x ^ y) ^ x = y
```

> **Multiplication:**  
> i \* 8;  
> i &lt;&lt; 3; \[8 = 2^3, so use 3\]
>
> **Division:**  
> i / 16;  
> i &gt;&gt; 4; \[16 = 2^4, so use 4\]
>
> **Modulo:**  
> i % 4;  
> i & 3; \[4 = 1 &lt;&lt; 2, apply \(\(1 &lt;&lt; 2\) - 1\), so use 3\]
>
> There are two shifts - logical shift & arithmetic shift. In right both are different for left both are same. Logical shift is normal. In arithmetic most significant signed bit is copied instead of zero. So a negative number remains negative.

### Repeating elements of array problem

Given array \[1, 2, 4, 2, 1\] finding xor 1^2^4^2^1 = xor\(1^1\)^\(2^2\)^\(4\) = 0^0^4 = 4

Given Array \[1, 1, 2, 2, 4, 5\] we need to find both 4 & 5  
 If we simply xor all numbers we will get 4^5 which will definitely be non zero. \(100\)^\(101\)=\(001\) Now if we divide the array elements into two one having 1 at unit place other having 0. \[1, 1, 5\], \[2, 2, 4\] take xor of both to get the ans  
 If 100\(4\) & 110\(6\) are non repeating numbers we need to divide the array based on tense \(check rightmost set bit pos\) place set or unset

```cpp
int arr[] = {1, 1, 2, 2, 3, 9};
int n = sizeof(arr) / sizeof(int);
int xors = 0;
for (int i = 0; i < n; ++i) xors ^= arr[i];

int temp = 0;
while(xors > 0)
{
    if (xors&1) break;
    ++temp;
    xors >>= 1;
}
int mask = 1<<temp;

int num1 = 0, num2 = 0;
for (int i = 0; i < n; ++i)
{
    if (((arr[i]&mask)>>temp)&1) num1 ^= arr[i];
    else num2 ^= arr[i];
}
cout << num1 << " " << num2 << endl;
```

Given array \[7, 7, 3, 4, 2, 4, 3, 3, 4, 7\] all numbers except one is occurring thrice we need to find that number.  
 add binary position values \(111 + 111 + 011 + 100 + 010 + 100 + 011 + 011 + 100 + 111\) = 676 %3 all digit = 010 = 0.20 + 1.21 + 0.22 = 2

```cpp
int arr[] = {7, 11, 3, 4, 9, 4, 3, 3, 4, 7, 9, 9, 7};
    int n = sizeof(arr) / sizeof(int);

    int count[32] {};
    for (int i = 0; i < n; ++i)
    {
        int cur = arr[i], pos = 0;
        while (cur > 0)
        {
            if (cur&1) ++count[pos];
            ++pos;
            cur >>= 1;
        }
    }
    int ans = 0;
    for (int i = 0; i < 32; ++i) ans += pow(2, i) * (count[i] % 3);
    cout << ans << endl;
```

### Calculating number of set bits

```cpp
int countBits(int n)
{
    //Time: O(number of bits)
    int count = 0;
    while (n > 0)
    {
        count += (n&1);
        n = n>>1;
    }
    return count;

    //Time: O(number of set bits)
    int count = 0;
    while(n)
    {
        ++count;
        n = n&(n-1);
    }

    // __builtin_popcount(n); or __builtin_popcountl or __builtin_popcountll
}
```

### Get Set Clear ith bit

```cpp
int getIthBit(int n, int i)
{
    return (n&(1<<i)) == 0 ? 0 : 1;
}
void setIthBit(int &n, int i)
{
    n = n|(1<<i);
}
void clearBit(int &n, int i)
{
    n = n&(~(1<<i));
}

#define has_bit(bit_mask, x) ((bit_mask) & (1ULL << (x)))
#define turn_on_bit(bit_mask, x) (bit_mask |= (1ULL << (x)))
#define turn_off_bit(bit_mask, x) (bit_mask &= (~(1ULL << (x))))
#define smallest_on_bit(bit_mask) (__builtin_ctzint((bit_mask) & (-(bit_mask))))
```

### Hamming Distance

hamming\(a,b\) between two strings a and b of equal length is the number of positions where the strings differ

```cpp
// Naive way
int hamming(string a, string b)
{
    int d = 0;
    for (int i = 0; i < k; i++)
        if (a[i] != b[i]) d++;
    return d;
}
/* Given a list n bit strings of length k, time complexity will be O(n^2 k)
If k is small we can store it as integers and calculating hamming distance */
int hamming(int a, int b) { return __builtin_popcount(a^b); }
```

### Counting Subgrids

![Given a binary grid, count how many subgrid have corners \(4 corners\) as 1](.gitbook/assets/image%20%2886%29.png)

```cpp
/* Naive O(n^3) approach, picks 2 rows i & j then iterate all
columns and check if all corners are 1 */
for (int i = 0; i < n; ++i)
    for (int j = 0; j < n; ++j)
        for (int k = 0; k < m; ++k)
            if (grid[i][k] == 1 && grid[j][k] == 1) count++;

/* Bit Optimization O(n^3 / N) appraoch, continuing same idea of above we can
reduce internal for loop of N by dividing the grid into block of columns such
that each block consist of N consecutive columns. Then each row is stored as
a list of k-bit numbers */
for (int k = 0; k <= n/N; ++i)
    count += __builtin_popcount(grid[i][k] & grid[j][k]);

/*  a random grid of size 2500×2500 and compared the original and bit optimized
implementation. While the original code took 29.6 seconds, the bit optimized
version only took 3.1 seconds with N = 32 (int numbers) and 1.7 seconds with
N = 64 (long long numbers) */
```

### Super awesome technique to handle binary indexing

```cpp
/*
Say we are given a vector of pairs with start and end value. And also given
a data in form of binary (void*) we need to clip it (start, end) for each
i present in the vector.

Easy way to do it will be take a mask of 1s at that place and OR it with
data.
-1 is all 1s do shifting for final mask
((-1) << (start))              its    11111000
((-1) >> (end - start + 1))    its    00111111
and both of them we will get mask     00111000

But you know what this is bullshit XD a way easier solution is create an
array of this struct
*/
struct myStruct
{
    int x : (3 * 8);
    // This will automatically reserve continuous 24 bits or 3 bytes
};
```

### Insertion

You are given two 32-bit numbers N, M and two bit positions i and j. Write a method to insert M into N such that M starts at bit j and ends at bit i. You can assume that the bits j through i have enough space to fit all of M. That is if M = 10011, you can assume that there are at least 5 bits between j and i. You would not , for example, have j = 3 and i = 2, because M could not fully fit between bit 3 and bit 2.

Example: N = 10000000000, M = 10011, i = 2, j = 6  
Output: 10001001100

```text
Create a mask like this 1111100000111111 then and it with n
then shift m i bits <<
Then n | m will be ans
```

### Binary to String

Given a real number between 0 and 1 \(e.g. 0.72\) that is passed in as a double, print the binary representation. If the number cannot be represented accurately in binary with at most 32 characters, print 'ERROR'

```cpp
/* Say given 0.875 = 1*(2^-1) + 0*(2^-2) + 1*(2^-3)
So in binary it becomes 0.101*/
void printBinaryFloat(double num)
{
    assert(num >= 0 && num <= 1);
    string res = "0.";
    while (num)
    {
        if (res.size() > 32) { cout << "ERROR!\n"; return; }
        double x = num*2;
        if (x >= 1) { res += '1'; num = r - 1; }
        else { res += '0'; num = r; }
    }
    cout << res << '\n';
}
```

### Flip Bit to Win

You have an integer and you can flip exactly one bit from a 0 to 1. Write code to find the length of longest sequence of 1s you could create.

Input: 1775 \(11011101111\)  
Output: 8

```cpp
/* Easy solution is we create a prefSum from both side. Then iterate
bitstring if 0 comes we will basically try to flip it and max
there will be prefL[i-1] + 1 + prefR[i+1] */

/* Better approach is create (ex testacse):
[0, 4, 1, 3, 1, 2, 21] this array it starts for 0 (right to left
in binary) we have 0 zeroes then 4 ones then 1 zero then 3 ones
then 1 zero then 2 ones. */

int longestSeq(int n)
{
    vector<int> seq;
    bool onesTurn = false;
    int cnt = 0;
    while (n)
    {
        if (onesTurn ^ (n&1))
        {
            seq.push_back(cnt);
            onesTurn = !onesTurn;
            cnt = 1;
        }
        else cnt++;
        n >>= 1;
    }
    if (cnt) seq.push_back(cnt);
    seq.push_back(32 - accumulate(seq.begin(), seq.end(), 0));
    
    int res = 0;
    for (int i = 0; i < seq.size(); i += 2)
    {
        if (seq[i] == 1)
        {
            int cur = 1;
            if (i-1 >= 0) cur += seq[i-1];
            if (i+1 < seq.size()) cur += seq[i+1];
            res = max(res, cur);
        }
    }
    return res;
}

/* We can reduce O(N) space to O(1) idea is we only need 3 states
so do both task in one iteration while mainting only 3 states. */
```

### Closest smaller and greater numbers with same number of set bits

Given a positive integer, print the next smallest and the next largest number that have the same number of 1 bits in their binary representation.

```cpp
/* Next number: Find rightmost unset bit, and set it
Prev number: Find rightmost set bit, unset it and set all before

but we have to also maintain set bits count, so if we flip a set to
unset to compensate we must do vice versa to some other bit.

(Next Greater)
> Flip right most non-trailing zero
eg: 11011001111100
    11011011111100
> Clear bits to the right
    11011010000000
> Maintain set bit cnt from right size
    11011010001111

(Prev Smaller)
> Flip right most non-trailing one
eg: 10011110000011
    10011100000011
> Clear bits to the right
    10011100000000
> maintain count from left of chosen point (in step 1)
    10011101110000 */

// More optimized approach (TODO explanation)
void getNext(int x)
{
    int cntZeroes = 0, cntOnes = 0, cur = x;
    while (cur && !(cur&1)) // cnts no of trailing zeroes
        cntZeroes++, cur >>= 1;
    while (cur&1) // cnts immediate one set block
        cntOnes++, cur >>= 1;
    
    if (cntZeroes+cntOnes == (8*sizeof(x)-1) || cntZeroes+cntOnes == 0)
        return -1;
    
    return (x + (1<<cntZeroes) + (1<<(cntOnes - 1)) - 1);
}
void getPrev(int x)
{
    int cntZeroes = 0, cntOnes = 0, cur = x;
    while (cur&1) cntOnes++, cur >>= 1;
    if (cur == 0) return -1;
    while (cur && !(cur&1)) cntZeroes++, cur >>= 1;
    
    return (x - (1<<cntOnes) - (1<<(cntZeroes - 1)) + 1);
}
```

### Debugger

Explain what the following code does: \(\(n & \(n-1\)\) == 0\)

```text
n & (n-1) == 0
means that n and (n-1) never have a common set bit

say n is 1101011000
        -         1
       --------------
         1101010111
so first set bit from right gets unset and all before gets set.

so and of them == 0 means that the number is power of 2
```

### Conversion

Write a function to determine the number of bits you would need to flip to convert invert A to integer B.

```text
built_in_popcount(A ^ B)
```

### Pairwise Swap

Write a program to swap odd and even bits in an integer with as few instructions as possible \(e.g. bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped and so on\).

```cpp
int solve(int x)
{
    return ((x & 0xaaaaaaaa) | (x & 0x55555555));
    // 0xaaaaaaaa in bin -> 1010 1010 1010 1010 1010 1010 1010 1010
    // 0x55555555 in bin -> 0101 0101 0101 0101 0101 0101 0101 0101
}
```

### Draw Line

A monochrome screen is stored as a single array of bytes, allowing eight consecutive pixels to be stored in one byte. The screen has width w, where w is divisible by 8 \(that is, no byte will be split across rows\). The height of the screen, of course, can be derived from the length of the array and the width. Implement a function that draws a horizontal line from \(x1, y\) \(x2, y\).  
The method signature should look something like:  
`drawLine(byte[] screen, int width, int x1, int x2, int y)`

```cpp
/* Naive way is iterate (x1, x2) in yth row and set the pixel
we can optimize it by setting entire 8 bytes 0xFF if the gap is
large. */

void drawLine(byte[] screen, int width, int x1, int x2, int y)
{
    int startOffset = x1%8, firstFullByte = x1/8;
    if (startOffset != 0) firstFullByte++;
    int endOffset = x2%8, lastFullByte = x2/8;
    if (endOffset != 0) endFullByte++;
    
    for (int i = firstFullByte; i <= lastFullByte; ++i)
        screen[(width/8)*y + i] = (byte)0xFF;
    
    byte startMask = (byte)(0xFF >> startOffset);
    byte endMask = (byte)~(0xFF >> endOffset);
    
    if (x1/8 == x2/8) // x1 and x2 are same byte
        screen[(width/8)*y + (x1/8)] |= (byte)(startMask & endMask);
    else
    {
        if (startOffset != 0)
            screen[(width/8)*y + firstFullByte - 1] |= startMask;
        if (endOffset != 7)
            screen[(width/8)*y + lastFullByte + 1] |= endMask;
    }
}
```

### [Bitwise AND of numbers in range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

```cpp
int rangeBitwiseAnd(int m, int n)
{
    int res = 0;
    for (int i = 31; i >= 0; --i)        
    {
        bool p = (m>>i)&1, q = (n>>i)&1;
        if (!p && !q) continue;
        if (p && q) res += (1<<i);
        else break;
    }
    return res;
}
```

