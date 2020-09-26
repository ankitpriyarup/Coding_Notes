# Square Root Algorithms

### Simple Range Query-Update Problem \(Sum/Max/Min\)

If we divide our regular array of size n to blocks each having size  √n \(last block may have lesser elements\) then we can update in O\(1\) and query in O\(**√n\)**

```cpp
// build
int len = (int) sqrt(n + .0) + 1;
vec<1, int> blocks(len, 0);
for (int i = 0; i < n; ++i) blocks[i/len] += arr[i];

// update
int oldVal = arr[x]
arr[x] = val;
blocks[x/len] += val - oldVal;

// query: sum [l, r]
int l = 2, r = 5, res = 0;
int _l = l/len, _r = r/len;
if (_l == _r)
    for (int i = l; i <= r; ++i) res += arr[i];
else
{
    for (int i = l, end = ((_l+1)*len - 1); i <= end; ++i) res += arr[i];
    for (int i = _l+1; i <= _r-1; ++i) res += blocks[i];
    for (int i = _r*len; i <= r; ++i) res += arr[i];
}
```

Simmilarly we can find minimum and maximum using this.

### Case Processing

Given a 2D n x n grid, each cell is assigned a letter. Find two cells with same letter whose manhattan distance is minimum. In below case its 'E'.

![](.gitbook/assets/image%20%2861%29.png)

**Algorithm 1**: Go through all all pairs of cells with letter c \(let's say there are k blocks with c letter\) it will be K^2

**Algorithm 2**: Perform a breadth-first search that simultaneously starts at each cell with letter c. The minimum distance between two cells with letter c will be calculated in O\(n\) time.



