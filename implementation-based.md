# Implementation Based

### MinStack \(Stack Modification\)

```cpp
stack<pair<int, int>> st;

// Finding minimum
return st.top().second;

// Adding an element (newElem)
int newMin = st.empty() ? newElem : min(newElem, st.top().second);
st.push({newElem, newMin});

// Removing and element
int removedElement = st.top().first;
st.pop();
```

### MinStack \(Queue Modification\)

Store only those items which are needed to determine the minimum \(Monotonic queue\). All the elements that we removed can never be a minimum itself, so this operation is allowed. 

When we extract minimum from \(original queue\) it might not be there since we removed it so we should check before removing.

```cpp
deque<int> q;

// Finding minimum
return q.front();

// Adding an element (newElem)
while (!q.empty() && q.back() > newElem) q.pop_back();
q.push_back(newElem);

// Removing an element
if (!q.empty() && q.front() == removeElement) q.pop_front();
```

Storing index of each element \(This doesn't require knowledge of removeElement\) making it independent of original queue

```cpp
deque<int> q;
int cntAdded = 0, cntRemoved = 0;

// Finding minimum
return q.front().first;

// Adding an element (newElem)
while (!q.empty() && q.back() > newElem) q.pop_back();
q.push_back({newElem, cntAdded});
cntAdded++;

// Removing an element
if (!q.empty() && q.front().second == cntRemoved) q.pop_front();
cntRemoved++;
```

### [Insert Delete Get Random O\(1\)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

Heap like idea of swapping with last element while deleting

```cpp
class RandomizedSet {
public:
    unordered_map<int, int> rec;
    vector<int> arr;
    default_random_engine seed;

    RandomizedSet()
    {
        rec.clear();
        arr.clear();
    }

    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val)
    {
        if (rec.find(val) != rec.end()) return false;
        rec[val] = arr.size();
        arr.push_back(val);
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val)
    {
        if (rec.find(val) == rec.end()) return false;
        arr[rec[val]] = arr.back();
        rec[arr.back()] = rec[val];
        arr.pop_back();
        rec.erase(val);
        return true;
    }
    
    /** Get a random element from the set. */
    int getRandom()
    {
        int rand = uniform_int_distribution<>(0, arr.size()-1)(seed);
        return arr[rand];
    }
};
```

### Matrix Multiplication

```cpp
for (int i = 0; i < A.n; i++)
{
    for (int j = 0; j < B.m; j++)
    {
        res[i][j] = 0;
        for (int k = 0; k < A.m; k++)
            res[i][j] += A[i][k] * B[k][j];
    }
}
```

Stressan's Divide and Conquer approach

![](.gitbook/assets/image%20%28309%29.png)

> Pad zeroes in case of non 2 power matrix

### Sparse Matrix Multiplication

* Each row can have &lt;= m number of elements. We will keep track of vals and cols of those non-zero entries.
* Size of non-zero elements in a row will be stored in rows \(see get function\)

```cpp
template<typename T>
class SparseMatrix
{
private:
    vector<T> *vals;
    vector<T> *rows, *cols;

public:
    int n, m;

    SparseMatrix(int _n, int _m)
    {
        assert(_n > 0 && _m > 0);
        n = _n, m = _m;
        vals = NULL, cols = NULL;
        rows = new vector<int>(n+1, 1);
    }
    ~SparseMatrix()
    {
        if (vals)
        {
            delete vals;
            delete cols;
        }
        delete cols;
    }
    T get(int i, int j) const
    {
        assert(i >= 1 && j >= 1 && i <= n && j <= m);
        for (int pos = ((*rows)[i-1] - 1); pos < ((*rows)[i] - 1); ++pos)
        {
            int curCol = (*cols)[pos];
            if (curCol == j) return (*vals)[pos];
            else if (curCol > j) break;
        }
        return T();
    }
    SparseMatrix& set(T val, int i, int j)
    {
        assert(i >= 1 && j >= 1 && i <= n && j <= m);
        int pos = (*rows)[i-1] - 1, curCol = 0;
        for (; pos < (*rows)[i] - 1; ++pos)
        {
            curCol = (*cols)[pos];
            if (curCol >= j) break;
        }
        if (curCol != j && val != T()) insert(pos, i, j, val);
        else if (val == T()) remove(pos, i);
        else (*vals)[pos] = val;
        return *this;
    }
    SparseMatrix<T> multiply(const SparseMatrix<T> &x) const
    {
        assert (m == x.n);
        SparseMatrix<T> res(n, x.m);
        for (int i = 1; i <= n; ++i)
        {
            for (int j = 1; j <= x.m; ++j)
            {
                T cur = T();
                for (int k = 1; k <= m; ++k)
                    cur += get(i, k) * x.get(k, j);
                res.set(cur, i, j);
            }
        }
        return res;
    }

private:
    void insert(int index, int i, int j, T val)
    {
        if (!vals)
        {
            vals = new vector<T>(1, val);
            cols = new vector<int>(1, j);
        }
        else
        {
            vals->insert(vals->begin() + index, val);
            cols->insert(cols->begin() + index, j);
        }

        for (int x = i; x <= n; ++x) (*rows)[x] += 1;
    }
    void remove(int index, int i)
    {
        vals->erase(vals->begin() + index);
        cols->erase(cols->begin() + index);

        for (int x = i; x <= n; ++x) (*rows)[x] -= 1;
    }
};
```

### [Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/)

```cpp
class NestedIterator {
public:
    stack<vector<NestedInteger>::iterator> front, back;
    NestedIterator(vector<NestedInteger> &nestedList)
    {
        front.push(nestedList.begin());
        back.push(nestedList.end());
    }
    
    int next()
    {
        return (front.top()++)->getInteger();
    }
    
    bool hasNext()
    {
        while (!front.empty())        
        {
            auto it = front.top();
            if (it == back.top())
            {
                front.pop(); back.pop();
                if (!front.empty()) front.top()++;
            }
            else if (it->isInteger()) return true;
            else
            {
                front.push(it->getList().begin());
                back.push(it->getList().end());
            }
        }
        return false;
    }
};
```

### [LRU Cache](https://leetcode.com/problems/lru-cache)

```cpp
/* LRU = Least Recently Used Cache
say a cache of size 4 we insert 0 then 1 then 2 then 3 i.e our cache becomes
3 2 1 0 if we insert another item size becomes 5 we can only have a capacity of 4
so while insertion least recently used gets deleted i.e. 4 3 2 1 [0 gets deleted]
If we access say 2 then it becomes 2 4 3 1 Now if we insert 5, 1 gets removed
i.e. 5 2 4 3 this get & set function should be in O(1) */

class LRUCache {
public:

    struct Node { int key, val; Node *prev, *next; };
    unordered_map<int, Node*> rec;
    Node *head, *tail;
    int sz, maxCapacity;

    LRUCache(int capacity)
    {
        head = NULL, tail = NULL;
        sz = 0, maxCapacity = capacity;
    }

    void put(int key, int value)
    {
        if (rec.find(key) == rec.end())
        {
            Node *newNode = new Node({key, value, NULL, NULL});
            rec[key] = newNode;
            if (!head && !tail) head = newNode, tail = newNode;
            else
            {
                newNode->next = head;
                head->prev = newNode;
                head = newNode;
            }
            sz++;
            if (sz > maxCapacity)
            {
                Node *toDel = tail;
                tail = tail->prev;
                if (tail) tail->next = NULL;
                rec.erase(toDel->key);
                delete (toDel);
                sz--;
            }
        }
        else
        {
            get(key);
            rec[key]->val = value;
        }
    }

    int get(int key)
    {
        auto it = rec.find(key);
        if (it != rec.end())
        {
            Node *newHead = it->second;
            if (newHead == head) return newHead->val;

            Node *p = newHead->prev, *q = newHead->next;
            if (p) p->next = q;
            if (q) q->prev = p;
            if (newHead == tail) tail = p;
            newHead->next = head;
            newHead->prev = NULL;
            head->prev = newHead;
            head = newHead;
            return newHead->val;
        }
        else return -1;
    }
};
```

### [Design Twitter](https://leetcode.com/problems/design-twitter/)

```cpp
class Twitter {
public:
    unordered_map<int, vector<pair<int, int>>> rec;
    unordered_map<int, unordered_set<int>> followers;
    int timer = 0;

    Twitter() { rec.clear(); followers.clear(); timer = 0; }
    void postTweet(int userId, int tweetId) { rec[userId].push_back({++timer, tweetId}); }
    void follow(int followerId, int followeeId)
    {
        if (followerId == followeeId) return;
        followers[followerId].insert(followeeId);
    }
    void unfollow(int followerId, int followeeId)
    {
        if (followerId == followeeId) return;
        followers[followerId].erase(followeeId);
    }
    vector<int> getNewsFeed(int userId)
    {
        vector<pair<int, int>> tmp;
        for (auto &y : rec[userId]) tmp.push_back(y);
        for (auto &x : followers[userId])
            for (auto &y : rec[x]) tmp.push_back(y);
        int sz = min(10, (int)tmp.size());
        partial_sort(tmp.begin(), tmp.begin()+sz, tmp.end(), greater<pair<int, int>>());

        vector<int> res;
        for (int i = 0; i < sz; ++i)
            res.push_back(tmp[i].second);
        return res;
    }
};
```

### [Find Median From Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

* Sort numbers find median, we can maintain sorted array while addNum it will take O\(logN\) time in lower\_bound and worst case all numbers have to be shifted so O\(NlogN\)

```cpp
// Avg case logN, but worst case linear
vector<int> arr;
int sz = 0;
MedianFinder() { }
void addNum(int num)
{
    sz++;
    if (arr.empty()) arr.push_back(num);
    else arr.insert(lower_bound(arr.begin(), arr.end(), num), num);
}
double findMedian()
{
    return sz&1 ? arr[sz/2] : (arr[sz/2 + 1] + arr[sz/2])/2.0;
}

// O(logN) using 2 heaps
priority_queue<int> l;
priority_queue<int, vector<int>, greater<int>> r;
void addNum(int num)
{
    l.push(num);
    r.push(l.top());
    l.pop();
    if (l.size() < r.size())
    {
        l.push(r.top());
        r.pop();
    }
}
double findMedian()
{
    return l.size() > r.size() ? l.top() : ((double) l.top() + r.top()) * 0.5;
}
```

* Maintain two heaps i.e. left and right half from middle. We are performing 3 push and 2 pop operations making it total O\(5logN\)

```cpp
priority_queue<int> left;
priority_queue<int, vector<int>, greater<int>> right;

MedianFinder() { }
/* Think 3 steps: sz(left) == sz(right), sz(left) > sz(right), sz(left) < sz(right) */
void addNum(int num)
{
    left.push(num);
    right.push(left.top());
    left.pop();

    if (left.size() < right.size())
    {
        left.push(right.top());
        right.pop();
    }
}
double findMedian()
{
    return (left.size() > right.size()) ? left.top() : (left.top() + right.top())/2.0;
}
```

* We can use policy based data structure, both insertion and find takes logN & 3logN time here but there's no overhead of inserting 5 times so this might perform better in some scenerios

```cpp
orderedMultiSet st;
int n = 0;

MedianFinder() { }
void addNum(int num)
{
    st.insert(num);
    n++;
}
double findMedian()
{
    return n&1 ? *st.find_by_order(n/2) :
        (*st.find_by_order(n/2 - 1) + *st.find_by_order(n/2))/2.0;
}
```

### [Design HashMap](https://leetcode.com/problems/design-hashmap/)

```cpp
class MyHashMap {
private:
    int hashfunction(int key)
    {
        return key&127; // negative num % 128 may give negative val, but & 127 won't
    }
    vector<pair<int, int>> data[128];

public:
    MyHashMap() { for (auto &x : data) x.clear(); }
    void put(int key, int value)
    {
        int hash = hashfunction(key);
        for (auto &x : data[hash])
            if (x.first == key) { x.second = value; return; }
        data[hash].push_back({key, value});
    }
    int get(int key)
    {
        int hash = hashfunction(key);
        for (auto &x : data[hash])
            if (x.first == key) return x.second;
        return -1;
    }
    void remove(int key)
    {
        int hash = hashfunction(key);
        for (int i = 0; i < data[hash].size(); ++i)
            if (data[hash][i].first == key) { data[hash].erase(data[hash].begin() + i); break; }
    }
};
```

### [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

```cpp
class MyCalendar {
public:
    set<pair<int, int>> st1, st2;
    MyCalendar() { }
    bool book(int s, int e)
    {
        /*
            ---             case 1              -----               avoid case 1
          =======                                      ======
          
            ------          case 2                      ------      avoid case 2
          =====                                 ======
          
          ------            case 3
            ======
        
        --------------      case 4
            ======

            Above cases can be concluded in two lines:
            - find a booking with start atleast s+1 if it's start < e
              then it's an overlap
            - find a booking with end atleast s+1 if it's start < e  */
        
        auto it1 = st1.lower_bound({s+1, INT_MIN});
        if (it1 != st1.end() && it1->first < e) return false;
        auto it2 = st2.lower_bound({s+1, INT_MIN});
        if (it2 != st2.end() && it2->second < e) return false;

        st1.insert({s, e});
        st2.insert({e, s});
        return true;
    }
};
```

### Read N Characters Given Read4
The API: int read4(char *buf) reads 4 characters at a time from a file.
The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.
By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.
Note: The read function will only be called once for each test case.
```c++
int read(char *buf, int n)
{
    int index = 0;
    char r4[4];
    while (index < n)
    {
        int c = read4(r4);
        for (int i = 0; i < c && index < n; ++i)
            buf[index++] = r4[i];
        if (c < 4) break;
    }
    return index;
}
```

### Read N Characters Given Read4 - Call Multiple Times
```c++
/* Previously it was called only once so:
    "filetestbuffer"
    read(6)
    read(5)

Gives output: [6, buf = "filete"] [5, buf = "buffe"]
which is incorrect, now we want output as: [6, buf = "filete"] [5, buf = "stbuf"] */

queue<char> q;
int read(char *buf, int n)
{
    int index = 0;
    while (!q.empty() && index < n)
    {
        buf[index++] = q.front();
        q.pop();
    }
    if (index == n) return n;
    char r4[4];
    while (index < n)
    {
        int c = read4(r4);
        int i = 0;
        for (; i < c && index < n; ++i)
            buf[index++] = r4[i];
        while (i < c) q.push(r4[i++]);
        if (c < 4) break;
    }
    return index;
}
```

### [Encode and Decode Strings](https://www.lintcode.com/problem/encode-and-decode-strings/description)

```cpp
class Solution {
public:
    string encode(vector<string> &strs)
    {
        string header = "(", body = "";
        for (auto &x : strs)
        {
            header += to_string(x.size()) + ',';
            body += x;
        }
        header = header.substr(0, header.size()-1) + ')';
        return header + body;
    }
    vector<string> decode(string &str)
    {
        string header = str.substr(1, str.find(')')-1);
        stringstream ss(header);
        string tmp;
        vector<string> res;
        int i = str.find(')')+1;
        while (getline(ss, tmp, ','))
        {
            int sz = stoi(tmp);
            res.push_back(str.substr(i, sz));
            i += sz;
        }
        return res;
    }
};
```

### [Guess the Word](https://leetcode.com/problems/guess-the-word/)

We could use tries also but that would be less optimal \(can't guess in 10 options\)

```cpp
void findSecretWord(vector<string>& wordlist, Master& master)
{
    vector<string> words = wordlist;
    for (int i = 0; i < 10; ++i)
    {
        string guess = words[rand()%words.size()];
        int matches = master.guess(guess);
        if (matches == 6)
            break;
        vector<string> newWords;
        for (const string cur : words)
        {
            int curMatches = 0;
            for (int j = 0; j < 6; ++j)
                if (guess[j] == cur[j]) curMatches++;
            if (curMatches == matches)
                newWords.push_back(cur);
        }
        words = newWords;
    }
}
```

### [Minesweeper](https://leetcode.com/problems/minesweeper/)

```cpp
class Solution {
public:
    vector<pair<int, int>> directions = {{1, -1}, {1, 0}, {1, 1}, {-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}};
    bool valid(int x, int y, vector<vector<char>> b)
    {
		return x >= 0 && x < b.size() && y >= 0 && y < b[0].size();
	}
    void dfs(vector<vector<char>>& board, int i, int j)
    {
        if (!valid(i, j, board)) return;
        if (board[i][j] == 'E')
        {
            int cnt = 0;
            for (const auto dir : directions)
            {
                if (valid(i + dir.first, j + dir.second, board) && board[i + dir.first][j + dir.second] == 'M')
                    cnt++;
            }
            if (cnt > 0) board[i][j] = ('0' + cnt);
            else
            {
                board[i][j] = 'B';
                for (const auto dir : directions) dfs(board, i + dir.first, j + dir.second);
            }
        }
    }
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click)
    {
        if (board[click[0]][click[1]] == 'M') { board[click[0]][click[1]] = 'X'; return board; }
        dfs(board, click[0], click[1]);
        return board;
    }
};
```

### [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

```cpp
map<int, int> mp;
MyCalendarTwo() { mp.clear(); }
bool book(int start, int end)
{
    int cnt = 0;
    mp[start]++, mp[end]--;
    for (auto it = mp.begin(); it != mp.end(); ++it)
    {
        cnt += it->second;
        if (cnt == 3) { mp[start]--, mp[end]++; return false; }
    }
    return true;
}
```

### Custom std::vector

```cpp
#include <bits/stdc++.h>
using namespace std;

template<typename T>
class Vector
{
private:
    T* m_Data = nullptr;
    size_t m_Size, m_Capacity;

public:
    Vector() : m_Size(0), m_Capacity(0) { ReAlloc(2); }
    void pushBack(const T& value)
    {
        if (m_Size >= m_Capacity)
            ReAlloc(m_Capacity + m_Capacity/2); // growing fx depends on use case
        m_Data[m_Size++] = value;
    }
    void popBack()
    {
        if (m_Size > 0)
        {
            m_Size--;
            m_Data[m_Size].~T();
        }
    }
    T& operator[](size_t index)
    {
        assert(index >= 0 && index < m_Size);
        return m_Data[index];
    }
    size_t size() const { return m_Size; }
    ~Vector() { delete[] m_Data; }

    // Improvements
    // this new fxn moves temp data provided as arg and moves instead of copy
    void pushBack(T&& value)
    {
        if (m_Size >= m_Capacity)
            ReAlloc(m_Capacity + m_Capacity/2);
        m_Data[m_Size++] = std::move(value);
    }
    template<typename... Args>
    T& emplaceBack(Args&&... args)
    {
        if (m_Size >= m_Capacity)
            ReAlloc(m_Capacity + m_Capacity/2);
        m_Data[m_Size] = T(std::forward<Args>(args)...);
        return m_Data[m_Size++];
    }
    // End of Improvements

private:
    void ReAlloc(size_t newCapacity)
    {
        /*  Functions: Allocate new block of memory, copy/move existing
        data, delete old memory block */
        T* newBlock = new T[newCapacity];
        // don't use smart pointers when dealing such low level data structure stuff

        if (newCapacity < m_Size)
            m_Size = newCapacity;   // In case of shrinking (after pop)

        for (size_t i = 0; i < m_Size; ++i)
            newBlock[i] = m_Data[i];
            // improvement -> newBlock[i] = std::move(m_Data[i]);
        /* why not memcpy? memcpy is fine for simple flot int types but in custom
        classes it won't call copy constructor above ensures that it gets called */

        delete[] m_Data;
        m_Data = newBlock;
        m_Capacity = newCapacity;
    }
};

// For demo
struct point
{
    float x, y, z;
    point() {}
    point(float _x, float _y, float _z) : x(_x), y(_y), z(_z) { }
    ~point() { cout << "DESTROY\n"; }
    point& operator=(const point& other)
    {
        cout << "COPY\n";
        x = other.x, y = other.y, z = other.z;
        return *this;
    }
    point& operator=(point&& other)
    {
        cout << "MOVE\n";
        x = other.x, y = other.y, z = other.z;
        return *this;
    }
};

int main()
{
    Vector<point> arr;
    /* this arg is temp data, which gets constructed then copied then destroy
    instead have a T&& method in vector */
    arr.pushBack(point(1, 2, 3));
    arr.pushBack(point(9, 8, 7));
    arr.pushBack(point(0, 1, 0));
    arr.popBack();
    arr.emplaceBack(1, 2, 3);

    for (int i = 0; i < arr.size(); ++i)
        cout << arr[i].x << " " << arr[i].y << " " << arr[i].z << '\n';
    return 0;
}
```

### Snake Game

```cpp
class SnakeGame
{
private:
    size_t curFood;
    int width, height;
    vector<pair<int, int>> food;
    deque<pair<int, int>> snake;

public:
    SnakeGame(int _width, int _height, vector<pair<int, int>> _food) :
        curFood(0), width(_width), height(_height), food(_food)
    {
        snake.push_back({0, 0});
    }

    // direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down
    // return the game's score after the move. Return -1 if game over. 
    int move(string direction)
    {
        int snakeLen = snake.size();
        if (snakeLen > width*height) return -1;

        auto snakeHead = snake.front().first;
        if (direction == "U") snakeHead.first--;
        else if (direction == "D") snakeHead.first++;
        else if (direction == "L") snakeHead.second--;
        else if (direction == "R") snakeHead.second++;

        if (curFood < food.size() && snakeHead == food[curFood]) curFood++;
        else snake.pop_back();

        if (find(snake.begin(), snake.end(), snakeHead) != snake.end()) return -1;
        snake.push_front(snakeHead);

        return curFood;
    }
};
```

### [Text Editor]
```c++
class textEditor
{
private:
    stack<char> leftStk, rightStk;

public:
    void insertWord(char word[])
    {
        int i = 0;
        while (word[i] != '\0')
            insertCharacters(word[i++]);
    }
    void insertCharacters(char character)
    {
        leftStk.push(character);
    }
    bool deleteCharacter()
    {
        if (rightStk.empty()) return false;
        else rightStk.pop();
        return true;
    }
    bool backSpaceCharacter()
    {
        if (leftStk.empty()) return false;
        else leftStk.pop();
        return true;
    }
    void moveCursor(int position)
    {
        int leftSz = leftStk.size(), rightSz = rightStk.size();
        if (position < leftSz) moveLeft(position);
        else moveRight(position - leftSz);
    }
    void moveLeft(int position)
    {
        int leftSz = leftStk.size();
        while (position != leftSz)
        {
            rightStk.push(leftStk.top());
            leftStk.pop();
            leftSz--;
        }
    }
    void moveRight(int cnt)
    {
        int rightSz = rightStk.size(), i = 1;
        if (cnt > rightSz) cout << "Cannot move the cursor, right, to the specified position\n";
        else
        {
            while (i <= cnt)
            {
                leftStk.push(rightStk.top());
                rightStk.pop();
                i++;
            }
        }
    }
    void findAndReplaceChar(char find, char replace)
    {
        int cnt = 1, originalCharPos = leftStk.size();
        moveCursor(0);
        while (!rightStk.empty())
        {
            if (rightStk.top() == find)
            {
                deleteCharacter();
                insertCharacters(replace);
            }
            else moveCursor(cnt++);
        }
        moveCursor(originalCharPos);
    }
    void print()
    {
        int cnt = 1, originalCharPos = leftStk.size();
        moveCursor(0);
        while (!rightStk.empty())
        {
            cout << rightStk.top();
            moveCursor(cnt++);
        }
        moveCursor(originalCharPos);
        cout << '\n';
    }
};

int main()
{
    textEditor editor;
    editor.insertWord("Ankit"); editor.insertWord(" Priyarup"); editor.insertWord(" is"); editor.insertWord(" a"); editor.insertWord(" good"); editor.insertWord(" boy!");
    editor.insertCharacters(' '); editor.insertCharacters(':'); editor.insertCharacters(')');
    editor.print();
    editor.moveCursor(0);
    editor.insertWord("AP, ");
    editor.print();
    return 0;
}
```

### [Time Based Key Value Store](https://leetcode.com/problems/time-based-key-value-store/)

```cpp
unordered_map< string, set<pair<int, string>, greater<pair<int, string>>> > rec;
TimeMap() { }
void set(string key, string value, int timestamp)
{
    rec[key].insert({timestamp, value});
}
string get(string key, int timestamp)
{
    auto it = rec[key].lower_bound({timestamp+1, ""});
    if (it != rec[key].end()) return it->second;
    return "";
}
```



