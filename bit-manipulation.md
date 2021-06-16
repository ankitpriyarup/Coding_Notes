# Bit Manipulation

* In C++ every int is 32 bits.
* They are indexed from right to left.
* The bit representation of a paritcular number is either signed or unsigned.
* **Least Significat Bit (LSB) & Most Significant Bit (MSB):** 10 in binary is (MSB Part)1010 (LSB Part), so from MSB side the number is 1010 from LSB side number is 0101. Endiness (storing the actual data in the memory): Little Endian (LSB), Big Endian (MSB).
* 1's Complement is nothing but toggling every bit of the number. It is represented by using the symbol ~.
* 2's Compliment is : ~X + 1
* \_\_builtin\_clz(x): counts the number of zeros present in the leading position.
* \_\_builtin\_ctz(x): count the trailing zeros.
* (A + B) =  (A ^ B) + 2(A&B)

> In the above equation, it is the full adder.  Here A ^ B is the answer where as A&B is the carry. We are multiplying the A&B by 2 in order to left shift the bits. Instead of multiplying by 2 you can simply use left shift too. ( (A&B) << 1 )

```cpp
// Multplication of two numbers without using *

int multiply (int a, int b) {
    int ans  =  0;
    // loop is upto 32 because number of bits in int is 32 in cpp
    for(int i = 0; i < 32; i++) { // here wer are iterating till
    // b is 0
    // we can replace this loop by while(b!=0)
        if (b&1) ans += a; // if b is oodd add a to the answer
        b >>= 1; // right shifting the b or dividing by 2
        a <<= 1; // left shifting a or multiplying by 2
    }
    return ans;
}

/*
In simple terms it can explained as

while (b != 0) {
    if (b is odd) {
        result = result + a;
    }
    a = a * 2;
    b = b / 2;
}
*/

// if we want to avoid + operator then we can implement below code

int add(int x, int y) {
    while( y!= 0) {
        int carry = x & y;
        x =  x ^ y;
        y = carry << 1;
    }
    return x;
}

// same code can be recursively done as

int add(int x, int y) {
    if (y == 0) {
        return x;
    }

    return add(x^y, (x&y)<<1);
}

// Swapping of two numbers without using extra variable
int x = 10, y = 30;
x = x ^ y;
y = y ^ x;
x = x ^ y;

```

> **Multiplicaton**
> i * 8
> i << 3; [ as 2 power 3 is 8 , we are using 3]
>
> **Division**
> i / 16;
> i >> 4 [ as 2 power 4 is 16, so we use 4]
>
> **Modulo**
> i % 4;
> i & 3; [ 4 = 1 << 2, apply ((1 << 2) - 1), so we are using 3]
> There are two shift - logical shift & arithmetic shift. For Right shift both behave differently, left shift is same for logical and arithmetic. Logical shift is normal where as in arithmetic MSB signed bit is copied instead of zero, so that a negative number remains negative.

## Repeating elements of array problem

Given an array [1, 2, 4, 2, 1], finding xor of all the elements 1 ^ 2 ^ 4 ^ 2 ^ 1 = (1 ^ 1) ^ (2 ^ 2) ^ (4) = (0) ^ (0) ^ 4 = 4.

Given an array [1, 1, 2, 2, 4, 5] here we need to find both 4 and 5.
If we simply perform the xor operation then our answer will be 4^5 which will definitely be a non zero number (100) ^ (101) = (001). Now if we divide the array into two parts , one part will have 1 in the units place (LSB or Odd numbers), other part will have 0 in the units place (MSB or Even numebrs) i.e [1, 1, 5], [2, 2, 4] now by taking xor of both arrays separately we can find the answer.
Suppose if we have 2 even non repeating numbers then we need do divide the numbers based on the right most set bit position
Example in case of 4 (100) and 6 (110) we can go with the second bit position.

```cpp
int array[] = { 1, 1, 2, 2, 3, 9}; // elements of the array
// finding the size of the array using sizrof operator
int array_size = sizeof(array) / sizeof(int);
// variable to store the xor of all the values
int xor_value = 0;

// finding xor of all the values of the array
for(int i = 0; i < array_size; ++i) {
    xor_value ^= array[i];
}

// temp variable to find the first set bit from right side (LSB side)
int temp = 0;
while( xor_value > 0) {    
    if (xor_value & 1) // AND operation with last bit to check if it is set or not
        break;
    
    temp++; // increasing the counter
    xor_value >>= 1; // right shifting the number i.e (110) ==> (11)
}

int mask = 1 << temp; // creating  mask for the bit where we have found the first set bit in the xor value
int num1 = 0, num2 = 0;

for(int i = 0; i < n; ++i) {
    // here we performing xor on numbers differently based on the set bit present in the mask or not
    // that is set bit is there at temp position
    // we do xor with num1 else with num2
    if ((( array[i]&mask ) >> temp) & 1) num1 ^= array[i];
    else num2 ^= array[i];
}

// finally printing the numbers
cout << num1 << " " << num2 << endl; 
```

Given array [ 7, 7, 3, 4, 2, 4, 3, 3, 4, 7] all numbers except one is occuring thrice we need to find that number in the array.
If we add the binary positions of the bits [ 111 + 111 + 011 + 100 + 010 + 100 + 011 + 011 + 100 + 111] = 676.
676 % 3 = 010 (which is binary representation of the 2) 
we can convert this to decimal by 0*(2^2) + 1*(2^1) + 0*(2^0).

```cpp

int arr[] = { 7, 11, 3, 4, 9, 4, 3, 3, 4, 7, 9, 9, 7};
int arr_size = sizeof(arr)/sizeof(int);

int count[32] = {0};
// count array to store occurences of each bit at particular position

for(int i = 0; i < arr_size; ++i) {
    int cur = arr[i]; // storing the current element
    int pos = 0; // variable to store current position

    while(cur > 0 ) {
        if (cur & 1) 
            count[pos]++; // incrementing count of the set bit position
        
        pos++; // incrementing the value of position
        cur >>= 1; // performing right shift on the number
    }
}

int answer  = 0;
for(int i = 0; i < 32; ++i )
    answer += pow(2,i) * (count[i] % 3);

cout << answer << endl;
```

## Calculating Number of Set Bits

```cpp

int countBits(int n) {

    int count = 0;
    while (n > 0) {
        count += ( n&1 ); // n&1 is 1 only when LSB is 1
        n >>= 1; // performing right shift
    }
    return count;
}

// other way of counting set bits
int countBits(int n) {
    int count  = 0;

    while(n) {
        count++;
        n = n & (n-1);
    }
    return count;
}

// using builtin function
int countBits(int n) {
    return (int)__builtin_popcount(n);
}

```

## Count Number Of Bits

```cpp

// using logarithm
int totalBits(int n) {
    return (int)log2(n) + 1;
}

// by using the bit traversal
int totalBits(int n) {
    int count = 0;
    
    while(n) {
        count++;
        n >>= 1;
    }
    return count;
}
```

## Check Parity

* Parity of a number refers to number of 1's in the binary format of the number.
* If it contains odd number of ones then it is odd parity
* If it contains even number of ones then it is even parity
* In C++ there is builtin function which returns 1 for odd parity and 0 for even parity

```cpp
#include<bits/stdc++.h>
using namespace std;

int checkParity(int n) {
    return __builtin_parity(n);
}

int main() {
    int n;
    cin >> n;

    if (checkParity(n)) {
        cout << "Odd Parity" << endl;
    }
    else {
        cout << "Even Parity" << endl;
    }
}
```

## Count Number of Leading Zeros

* Example binary of 16 is 00000000 00000000 00000000 00010000
* It has 27 leading zeros ( as we discussed int is 32 bit all the leading ones are filled with 0 )

```cpp

int leadingZeros(int n) {
    return (int)__builtin_clz(n);
}
```

### Similarily we have function to count the trailing zeros

```cpp
int trailingZeros(int n) {
    return (int)__builtin_ctz(n);
}
```

### Pro Tip

* You can calculate 2 power something using left shift operator

```cpp

int power(n) {
    return 1 << n;
}
```

> Example
> 2 power 3 is 8 which is 1000
> 1 << 3 is ( 1 left shifted 3 times ) = 1000

* This trick can be used not only for power but also for multiplying and dividing by 2

```cpp
int multiply(int x) {
    return x << 1;
}

int divide(int x) {
    return x >> 1;
}
```

* Finding log to the base 2 of a 32 bit integer

```cpp
int log2(int x) {

    int result = 0;
    while( x >>= 1 ) {
        result++;
    }
    return result;
}
```

> **Logic:-** We right shift x repeatedly until it becomes 0 mean while we keep count on the shift operations. 
> This count value is the value of log to the base 2

## Get, Set & Clear ith bit

```cpp
int getIthbit(int n, int i) {
    return ( n & ( 1 << i) ) == 0 ? 0 : 1;
}

void setIthBit(int n, int i) {
    n = n | (1 << i);
}

void clearIthBit(int n, int i) {
    n = n & ( ~(1 << i));
}

#define has_bits(bit_mask, x) ((bit_mask) & (1ULL << (x)))
#define turn_on_bit(bit_mask, x) (bit_mask |= (1ULL << (x)))
#define turn_off_bit(bit_mask, x) (bit_mask &= (~( 1ULL << (x))))
#define smallest_on_bit(bit_mask) (__builtin_ctz(int)((bit_mask) & (~(bit_mask))))
```

## Hamming Distance

Hamming distance is a metric for comparing two binary data strings. While comparing two binary strings of equal length, Hamming distance is the number of bit positions in which the two bits differ

```cpp
// If the input is in string format the we can use below code snippet

int hammingDistance(string a, string b) {
    // assuming both strings are of equal length
    // their length is k
    int answer = 0;

    for(int i = 0; i < k; i++) {
        if (a[i] != b[i]) answer++;
    }
    return answer;
}

// If the input is in the form of numbers then XOR operation can be used 

int hammingDistance(int a, int b) {
    return __builtin_popcount(a^b);
}
```

## Counting Sub Grids Problem

![Given a binary grid, count how many subgrid have corners \(4 corners\) as 1](https://ieee.nitk.ac.in/blog/assets/img/Bit-Manipulation/grid2.png)

Given an n x n grid whose each square is either black (1) or white (0), calculate the number of subgrids whose all corners are black


There is a O(n^3) time algorithms for solving the problem; i.e go throgh all the n^2 pairs of rows and for each pair (a,b) calculate the number of columns that contains a black square in both rows in O(n) time

```cpp
// Naive approach, picks 2 rows i & j then iterate all
// columns and check if all corneres are 1

for(int i  = 0; i < n; i++) {
    for(int j = 0; j < n; j++) {
        for(int k = 0; k < m; k++) {
            if (grid[i][k] == 1 && grid[j][k] == 1) count++;
        }
    }
}

// complete functional snippet of above one is

int count_subgrid(const int **color, int n) {
    int subgrids = 0;
    for(int a = 0; a < n; a++) {
        for(int b = a+1; b < n; b++) { // loop over pairs(a,b)of rows
            int count = 0;
            for(int i = 0; i < n; i++) { // loop over all columns
                if (color[a][i] == 1 && color[b][i] == 1) {
                    count++;
                }
            }
            subgrids += ((count-1) * count)/2;
        }
    }
    return subgrids;
}

```

Bit optimization O(n^3/N) appraoch, continuing same idea of above we can reduce internal for loop of N by dividing the grid into block of columns such that each block consist of N consecutive columns. Then each row is stored as a list of K-bit numbers.

```cpp
for(int k = 0; k <= n/N; ++i) {
    count += __builtin_popcount(grid[i][k] & grid[j][k]);
}

/* a random grid of size 2500 x 2500 and compared the original bit optimized implementation.
While the original code took 29.6 seconds, the bit optimized version only took 3.1 seconds with N = 32 (int number) and 1.7 second with N = 64 (long long numbers) */
```

## Awesome technique to handle binary indexing

```cpp
/*
Say we are given a vector of pairs with start and end value. And also given 
a data in form of binary (void *) we need to clip it (start, end) for each
i present in the vector

Easy way to do it will be take a mask of 1st at that place and OR it with 
data.
-1 is all 1's do shifting for final mask
i.e -1 is binary is 11111111
((-1) << (start))           its (assuming start is 3)  11111000
((-1) >> (end - start + 1)  its (assuming end is 6)    00111111
AND both of them we will get mask                      00111000

But there is also a easier solution by creating an array of this struct
*/

struct myStruct {
    int x: ( 3 * 8 );
    // This will automatically reserve continuous 24 bits or 3 bytes

    // when we represent int var_name: some digit
    // then we are confining that var to digit number of bits
};
```

## Insertion

You are given two 32-bit numbers N, M and two bit positions i and j. Write a method to insert M into N such that M starts at bit j and ends  at bit i. You can assume that the bits j through i have enough space to fit all of M. That is if M = 10011, you can assume that there are atleast 5 bits between i and j. You will not get any example as j = 3 and i = 2, because M could not fully fit between bit 3 and bit 2.

Example: N = 10000000000, M = 10011, i = 2 and j = 6
Output: 10001001100

> Create a mask like 1111100000111111, after  that AND it with N
> Above step is to make bits in between i and j 0 and keep remaining same
>
> Then perform left shift on M , i times so that the bits of M reach the correct position
>
> Finally N | M (N OR M) will be the answer

## Binary String

Given a real number between 0 and 1 (e.g 0.72) that is passed in as a double, print the binary representation. If the number cannot be represented accurately in binary with at most 32 characters, then print ERROR

```cpp
/*
Suppose the given number is 0.875 = 1*(2^-1) + 0*(2^-2) + 1*(2^-3)
so in binary it becomes 0.101
*/

void printBinaryFloat(double num) {

    assert(num >= 0 && num <= 1)
    string res = "0.";

    while(num) {
        if (res.size() > 32) {
            cout << "ERROR";
            return;
        }
        double x = num * 2;
        if (x >= 1) {
            res += '1';
            num = x - 1;
        }
        else {
            res += '0';
            num = x;
        }
    }
    cout << res << endl;
}
```

## Flip Bit To Win

You have an integer and you can flip exactly one bit from a 0 to 1. Write a code to find the length of longest sequenceof 1s you could create 

Input: 1775 (11011101111)
Output: 8

```cpp
/*
Easy solution is we create a prefSum from both side. Then iterate
bitstring if 0 comes we will basically try to flip it and max
there will be prefL[i-1] + 1 + prefR[i+1]

Better Approach
[ 0, 4, 1, 3, 1, 2, 21] this array it starts for 0 ( right to left
in binary) we have 0 zeors, then we have 4 ones, then 1 zero, then 3 ones, then 1 zero and 2 ones
last is just 32 - number of bits used 
*/

int longestSequence(int n) {
    
    vector<int> sequence;
    bool onesTurn = false;
    int count = 0;

    while(n) {
        // this loop will create the sequence vector as discusseded above
        if (onesTurn ^ (n&1)) {
            sequence.push_back(count);
            onesTurn = !onesTurn;
            count = 1;
        }
        else {
            count++;
        }
        n >>= 1;
    }

    if (count) sequence.push_back(count);
    sequence.push_back(32 - accumulate(sequence.begin(), sequence.end(), 0));

    int result  = 0;
    // now we are looping through the sequence vector
    // here we are only checking the positions where we have 
    // stored the count of 0
    for(int i = 0; i < sequence.size(); i += 2) {
        // if count of 0 is 1 then add its previous and next value
        if (sequence[i] == 1) {
            int cur = 1;
            if (i-1 >= 0) cur += sequence[i-1]; 
            if (i+1 < sequence.size()) cur += sequence[i+1];
            result = max(result,cur);
        }
    }
    return result;
}

/*
Here we can reduce the O(N) space to O(1) idea is we only need 3 states
so do both task in one iteration while mainting only 3 states 
*/

// There is a small disadvantage in the above code
// It works only when there is one 0 is present between two sets of 1

// This can be overcomes by tweaking the above code little bit

int result = 0;

// traversing the sequence array or in this case vector
for(int i = 0; i < sequence.size(); i += 2 )
{
    // if there is only one zero add previous count of 1 and next count of 1
    if (sequence[i] == 1) {
        int cur = 1;
        if (i-1 >= 0) cur += sequence[i-1];
        if (i+1 < sequence.size()) cur += sequence[i+1];
        result = max(result,cur);

    }

    else if (sequence[i] > 1 ){
        int cur = 1;
        // suppose sequence vector is
        // [4 ones, 3 zeros, 5 ones]
        if ( i - 1 >= 0 ) result = max(result, cur + sequence[i-1]);
        // that is max of current result and 1 + prev count of 1
        if ( i + 1 <= sequence.size()) result = max(result, cur + sequence[i+1]);
        // that is max of current result and 1 + next count of 1
        // Here we are adding 1 because we are only allowed to flip one bit
    }
}

cout << result << endl;

```

## Closest smaller and greater number with same number of set bits

Given a positive integer, print the next smallest and the next largest number that have the same number of 1 bits in their binary representation

```cpp
/*
Next Number: Find the right most unset bit,set it and all the bits right to that must be flipped
i.e if the number is 13 (1101) for next number if change 2 bit to 1 and all the bits next to it are flipped 14 (1110)

Prev Number: Find the right most set bit, unset it and set all before

Here we need to remember that we should also keep the count of set bits after flipping them

(next greater with same no of set bits)
> Flip the right most non-trailing zero
eg: 11011001111100
    11011011111100
> Now clear bits to the right of it
    11011010000000
> Maintain set bit count from right side
    11011010001111

(prev smaller with same no of set bits)
> Flip the right most non-trailing one
eg: 10011110000011
    10011100000011
> Clear the bits to the right of it
    10011100000000
> Maintain count from left of chosen point in step 1
    10011101110000
*/

// Optimized code for the problem 
void getNext(int x) {
    int countZero = 0, countOne = 0, cur = x;

    while(cur && !(cur&1)) { // count no of trailing zeros
        countZero++;
        cur >>= 1;
    }

    while(cur&1) { // counts the immediate block of set bits
        countOne++;
        cur >>= 1;
    }

    if(countZero + countOne == (8*sizeof(x)-1)) {
        return -1;
    }

    return ( x + ( 1 << countZero) + ( 1 << (countOne -1)) -1);
}

void getPrev(int x) {
    int countZero = 0, countOne = 0, cur = x;
    
    while(cur&1) {
        countOne++, cur >>= 1;
    }

    if (cur == 0){
        return -1;
    }

    while(cur && !(cur&1)) {
        countZero++, cur >>= 1;
    }

    return (x - ( 1 << countOne) - ( 1 << (countZero-1)) + 1);
}

```

## Debugger

Explain what the following code does: (n & (n-1)) == 0

```
n & (n-1) == 0
it means that n and n-1 doesn't have any common set bit

it is generally used to check whether n is power of 2 or not

similarily we can check n is power of 4 or not by checking

(n & (n-1)) == 0 && (n&0xAAAA) == 0)
here is A is hexadecimal 1010 where 1 is present at every even position
where as in power of 4 , there should be only one set bit that too in the odd position
```

## Conversion

Write a function to determine the number of bits you would need to flip to convert A to integer B

```cpp
__builtin_popcount(A^B);
```

## Pairwise Swap

Write a program to swap odd and even bits in an integer with as few instructions as possible.
e.g bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped and so on

```cpp

int solve(int x) {
    return (( x & 0xaaaaaaaa) | ( x & 0x55555555));
    // 0xaaaaaaaa in bin -> 1010 1010 1010 1010 1010 1010 1010 1010
    // 0x55555555 in bin -> 0101 0101 0101 0101 0101 0101 0101 0101

```

## Draw Line

A monochrome screen is stored as a single array of bytes, allowing eight consecutive pixels to be stored in one byte. The screen has width w, where w is divisible by 8(that is, no byte will be split across rows). The height of the screen, of course, can be derived from the length of the array and the width. Implement a function that draws a horizontal line from (x1, y) (x2, y).
the method signature should look  something like:

`drawLine(byte[] screen, int width, int x1, int x2, int y)`

```cpp
/*
Naive approach is ofcourse iterate (x1, x2) in yth row and set the pixel
we can optimize it by setting entire 8 bytes 0xFF if the gap is large
*/

void drawLine(byte[] screen, int width, int x1, int x2, int y) {
    int startOffset = x1%8;
    int firstFullByte = x1/8;
    if (startOffset != 0) firstFullByte++;

    int endOffset = x2%8;
    int lastFullByte = x2/8;
    if (endOffset != 0) lastFullByte++;

    for(int i = firstFullByte; i <= lastFullByte; ++i) {
        screen[(width/8)*y + i] = (byte)0xFF;
    }

    byte startMask = (byte)(0xFF >> startOffset);
    byte endMask = (byte)~(0xFF >> endOffset);

    if (x1/8 == x2/8) // x1 and x2 are same byte
        screen[(width/8)*y + (x1/8)] |= (byte)(startMask & endMask);
    
    else {
        if (startOffset != 0) 
            screen[(width/8)*y + firstFullByte - 1] |=startMask;
        
        if (endOffset != 7)
            screen[(width/8)*y + lastFullByte + 1] |= endMask;
    }
}
```
### [Bitwise AND of numbers in range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

```cpp

int rangeBitwiseAnd(int m, int n) {
    int res = 0;
    for(int i = 31; i >= 0; i--) {
        bool p = (m >> i), q = (n >> i) & 1;
        if (!p && !q) continue;
        if (p && q) res += ( 1 << i );
        else break;
    }

    return res;
}

// another way of writing above function is

int rangeBitwiseAnd(int m, int n) {
    int answer = 0;
    for(int bit = 30; bit >= 0; bit--) {
        // here we have ignored the sign bit thats why loop is from 30

        if (m & (1 << bit) != (n & (1 << bit))) {
            break;
        }
        else {
            answer |= (m & (1 << bit));
        }
    }
    return answer;
}

```
