# Monotonic Structures

## Arithematic Evaluation

{% tabs %}
{% tab title="Algo" %}
```text
// Infix to postfix
> Push left paranthesis on stack, add right paranthesis to expression.
> Scan left to right and repeat.
> If operand then append in postfix expression otherwise push it on stack.
> If an operator with higher precidence (BODMAS) is already present in stack
    when pushing above then pop it and append to expression. If paranthes is
    close then pop and append insides.

// Infix to prefix
> Scan right to left. In the end reverse the expression. The behavior of
    paranthesis will also change. () will become )( during traversal

// Prefix to infix
> Scan right to left.
> If operand push on stack otherwise s1 = pop s2 = pop push (s1 operator s2)
    on the stack again.
> Pop the stack to get the output

// Postfix to infix
> Scan left to right.
> If operand push on stack otherwise s1 = pop s2 = pop push (s1 operator s2)
    on the stack again.
> Pop the stack and reverse it to get output

// Postfix to prefix
> Scan left to right.
> If operand push on stack otherwise s1 = pop s2 = pop push (operator s2 s1)
    on the stack again.

// Prefix to postfix
> Scan right to left.
> If operand push on stack otherwise s1 = pop s2 = pop push (s1 s2 operator)
    on the stack again.

// Evaluation of postfix
> Scan left to right
> If operand push on stack otherwise s1 = pop s2 = pop push evaluation of
    (s2 operator s1) on the stack again.
```
{% endtab %}

{% tab title="Code" %}
```cpp
#include <iostream>
#include <stack>
#include <algorithm>        //For reverse
#include <math.h>           //For pow
using namespace std;

int getOperatorRank(char oper)
{
    switch(oper)
    {
        case '^' : return 5;
        case '/' : return 4;
        case '*' : return 3;
        case '+' : return 2;
        case '-' : return 1;
    }
    return -1;
}

double evaluate(double a, double b, char oper)
{
    switch(oper)
    {
        // Everything in reverse b/a b^a b-a because it's postfix
        case '^' : return pow(b, a);
        case '/' : return b/a;
        case '*' : return b*a;
        case '+' : return b+a;
        case '-' : return b-a;
    }
    return 0;
}

int main()
{
    //Infix
    string expression = "5+(6*7-(7/8-9)*4)*3";
    expression = "(" + expression + ")";

    //INFIX TO POSTFIX
    stack<char> postfixStack;
    string postfix = "";
    for (int i = 0; i < expression.size(); ++i)
    {
        char cur = expression[i];
        if (cur == '+' || cur == '-' || cur == '*' || cur == '/' || cur == '^')
        {
            while (getOperatorRank(postfixStack.top()) > getOperatorRank(cur))
            {
                postfix += postfixStack.top();
                postfixStack.pop();
            }
            postfixStack.push(cur);
        }
        else if (cur == ')')
        {
            while (postfixStack.top() != '(')
            {
                postfix += postfixStack.top();
                postfixStack.pop();
            }
            postfixStack.pop();
        }
        else if (cur == '(') postfixStack.push(cur);
        else if (cur != ' ') postfix += cur;
    }
    cout << "postfix: " << postfix << endl;

    //INFIX TO PREFIX
    stack<char> prefixStack;
    string prefix = "";
    for (int i = expression.size()-1; i >= 0; i--)
    {
        char cur = expression[i];
        if (cur == '+' || cur == '-' || cur == '*' || cur == '/' || cur == '^')
        {
            while (getOperatorRank(prefixStack.top()) > getOperatorRank(cur))
            {
                prefix += prefixStack.top();
                prefixStack.pop();
            }
            prefixStack.push(cur);
        }
        else if (cur == '(')
        {
            while (prefixStack.top() != ')')
            {
                prefix += prefixStack.top();
                prefixStack.pop();
            }
            prefixStack.pop();
        }
        else if (cur == ')') prefixStack.push(cur);
        else if (cur != ' ') prefix += cur;
    }
    reverse(prefix.begin(), prefix.end());
    cout << "prefix: " << prefix << endl;

    //PREFIX TO INFIX
    stack<string> pre2inStack;
    string pre2in = "";
    for (int i = prefix.size()-1; i >= 0; i--)
    {
        char cur = prefix[i];
        if (cur == '+' || cur == '-' || cur == '*' || cur == '/' || cur == '^')
        {
            string a = pre2inStack.top();
            pre2inStack.pop();
            string b = pre2inStack.top();
            pre2inStack.pop();
            pre2inStack.push(a + string(1, cur) + b);
        }
        else if (cur != ' ') pre2inStack.push(string(1, cur));
    }
    pre2in = pre2inStack.top();
    cout << "prefix to infix: " << pre2in << endl;

    //POSTFIX TO INFIX
    stack<string> post2inStack;
    string post2in = "";
    for (int i = 0; i < postfix.size(); ++i)
    {
        char cur = postfix[i];
        if (cur == '+' || cur == '-' || cur == '*' || cur == '/' || cur == '^')
        {
            string a = post2inStack.top();
            post2inStack.pop();
            string b = post2inStack.top();
            post2inStack.pop();
            post2inStack.push(a + string(1, cur) + b);
        }
        else if (cur != ' ') post2inStack.push(string(1, cur));
    }
    post2in = post2inStack.top();
    reverse(post2in.begin(), post2in.end());
    cout << "postfix to infix: " << post2in << endl;

    //POSTFIX TO PREFIX
    stack<string> post2preStack;
    string post2pre = "";
    for (int i = 0; i < postfix.size(); ++i)
    {
        char cur = postfix[i];
        if (cur == '+' || cur == '-' || cur == '*' || cur == '/' || cur == '^')
        {
            string a = post2preStack.top();
            post2preStack.pop();
            string b = post2preStack.top();
            post2preStack.pop();
            post2preStack.push(string(1, cur) + b + a);
        }
        else if (cur != ' ') post2preStack.push(string(1, cur));
    }
    post2pre = post2preStack.top();
    cout << "postfix to prefix: " << post2pre << endl;

    //PREFIX TO POSTFIX
    stack<string> pre2postStack;
    string pre2post = "";
    for (int i = prefix.size()-1; i >= 0; i--)
    {
        char cur = prefix[i];
        if (cur == '+' || cur == '-' || cur == '*' || cur == '/' || cur == '^')
        {
            string a = pre2postStack.top();
            pre2postStack.pop();
            string b = pre2postStack.top();
            pre2postStack.pop();
            pre2postStack.push(a + b + string(1, cur));
        }
        else if (cur != ' ') pre2postStack.push(string(1, cur));
    }
    pre2post = pre2postStack.top();
    cout << "prefix to postfix: " << pre2post << endl;

    //EVALUATION OF POSTFIX
    stack<double> evaluateStack;
    double solution = 0;
    for (int i = 0; i < postfix.size(); ++i)
    {
        char cur = postfix[i];
        if (cur == '+' || cur == '-' || cur == '*' || cur == '/' || cur == '^')
        {
            double a = evaluateStack.top();
            evaluateStack.pop();
            double b = evaluateStack.top();
            evaluateStack.pop();
            evaluateStack.push(evaluate(a, b, cur));
        }
        else if (cur != ' ') evaluateStack.push(cur - '0');
    }
    solution = evaluateStack.top();
    cout << "Solution: " << solution << endl;
    return 0;
}
```
{% endtab %}
{% endtabs %}

### Basic Calculator
- https://leetcode.com/problems/basic-calculator/
- https://leetcode.com/problems/basic-calculator-ii/
- https://www.lintcode.com/problem/basic-calculator-iii/description

```cpp
// Other 2 variants can be solved easily by commenting some parts from this code
// Variant - III (+, -, *, /, (, ), empty_spaces and non-negative_numbers)
int calculate(string s)
{
    int num = 0, curRes = 0, res = 0;
    char op = '+';
    for (int i = 0; i < s.size(); ++i)
    {
        char ch = s[i];
        if (ch >= '0' && ch <= '9') num = num*10 + (ch-'0');
        else if (ch == '(')
        {
            int j = i, cnt = 0;
            while (i < s.size())
            {
                if (s[i] == '(') ++cnt;
                if (s[i] == ')') --cnt;
                if (cnt == 0) break;
                ++i;
            }
            num = calculate(s.substr(j+1, i-j-1));
        }
        if (ch == '+' || ch == '-' || ch == '*' || ch == '/' || i == s.size()-1)
        {
            switch (op)
            {
                case '+': curRes += num; break;
                case '-': curRes -= num; break;
                case '*': curRes *= num; break;
                case '/': curRes /= num; break;
            }
            if (ch == '+' || ch == '-' || i == s.size()-1)
                res += curRes, curRes = 0;
            op = ch, num = 0;
        }
    }
    return res;
}
```

## Implementations

### Stack using Array

```cpp
#define MAX 5

int stackArr[MAX], top = -1;
bool isEmpty() { return (top == -1); }
bool isFull() { return (top == MAX-1); }
void push(int val)
{
    if (isFull())
    {
        cout << "Stack is Full!" << endl;
        return;
    }
    ++top;
    stackArr[top] = val;
}
void pop()
{
    if (isEmpty())
    {
        cout << "Stack is Empty!" << endl;
        return;
    }
    --top;
}
int top()
{
    if (isEmpty())
    {
        cout << "Stack is Empty!" << endl;
        return -1;
    }
    return stackArr[top];
}
```

### Stack using linked list

```cpp
struct Node { int data; Node* next; } *top = NULL;
bool isEmpty() { return top == NULL; }
void push(int value)
{
    Node* temp = new Node{value, NULL};
    if (top != NULL)
        temp->next = top;
    top = temp;
}
void pop()
{
    if (isEmpty())
    {
        cout << "Stack is Empty!" << endl;
        return;
    }
    temp = top;
    top = top->next;
    delete temp;
}
int top()
{
    if (isEmpty())
    {
        cout << "Stack is Empty!" << endl;
        return;
    }
    return top->data;
}
```

### Queue using Array

```cpp
#define MAX 5
int qArr[MAX], front = -1, rear = -1;
bool isEmpty() { return (front == -1); }
bool isFull() { return (rear == MAX-1); } //In circular queue - (front==0 && rear==MAX-1) || (front == rear+1))
void enqueue(int val)
{
    if (isFull())
    {
        cout << "Queue is Full!" << endl;
        return;
    }
    if (front == -1 && rear == -1)
    {
        front = 0;
        rear = 0;
    }
    else ++rear;    //In Circular queue rear = (rear+1) % MAX;
    qArr[rear] = val;
}
void deqeue()
{
    if (isEmpty())
    {
        cout << "Queue is Empty!" << endl;
        return;
    }
    if (front == rear)
    {
        front = -1;
        rear = -1;
    }
    else ++front;   //In Circular queue front = (front+1) % MAX;
}
int front()
{
    if (isEmpty())
    {
        cout << "Queue is Empty!" << endl;
        return -1;
    }
    return qArr[front];
}
```

> Simmilarly Queue using linked list

### Priority Queue using Linked List \(Min prior\)

```cpp
struct Node { int data, prio; Node* next; } *root = NULL;
bool isEmpty() { return (root == NULL); }
void push(int val, int _prio)
{
    Node* newNode = new Node{val, _prio, NULL};
    if (root == NULL) root = newNode;
    else if (root->prio > newNode->prio)    //Flip sign for max prio
    {
        newNode->next = root;
        root = newNode;
    }
    else
    {
        Node* temp = root;
        //Flip sign for max prio
        while(temp->next != NULL && temp->next->prio < newNode->prio)
            temp = temp->next;
        newNode->next = temp->next;
        temp->next = newNode;
    }
}
void pop()
{
    if (isEmpty())
    {
        cout << "Queue is Empty!" << endl;
        return;
    }
    Node* temp = root;
    root = root->next;
    delete temp;
}
int peek()
{
    if (isEmpty())
    {
        cout << "Queue is Empty!" << endl;
        return -1;
    }
    return root->data;
}
```

### Priority Queue \(min prio\) using Doubly Linked List

```cpp
struct Node { int data, prio; Node *next, *prev; } *front = NULL, *rear = NULL;
bool isEmpty() { return (front == NULL && rear == NULL); }
void push(int val, int _prio)
{
    Node* newNode = new Node{ val, _prio, NULL, NULL };
    if (front == NULL && rear == NULL)
    {
        front = newNode;
        rear = newNode;
    }
    else if (newNode->prio <= front->prio)
    {
        //Front Insert
        newNode->next = front;
        front->prev = newNode->next;
        front = newNode;
    }
    else if (newNode->prio > rear->prio)
    {
        //Rear Insert
        newNode->next = NULL;
        rear->next = newNode;
        newNode->prev = rear;
        rear = newNode;
    }
    else
    {
        Node* temp = front;
        while(temp->next != NULL && temp->next->prio < newNode->prio)
            temp = temp->next;
        newNode->next = temp->next;
        newNode->prev = temp;
        temp->next = newNode;
        (temp->next)->prev = newNode;
    }
}
void pop()
{
    if (isEmpty())
    {
        cout << "Queue is Empty!" << endl;
        return;
    }
    Node* temp = front;
    front = front->next;
    delete temp;
}
int peek()
{
    if (isEmpty())
    {
        cout << "Queue is Empty!" << endl;
        return -1;
    }
    return front->data;
}
```

### Three In One

Describe how you could use a singly array to implement three stacks

```cpp
/* Easy solution is to simply have [0, n/3) [n/3, 2n/3) [2n/3, n) section specific
for each stack. But obvious disadvantage is that one stack may end up using all
space while other stack might be empty.
s = {0, 0, 0, 0, 0, 0}
     ^     ^     ^

If it was just 2 stacks we could have had picked 2 ends and variably modify
their lengths. We can make 3rd pointer from end here also (in 3 stack case) so:
s = {0, 0, 0, 0, 0, 0}
     ^     ^        ^
This should be somewhat more optimized

Generalized K stacks in 1
Most optimized way:
arr: [ 0  0  0  0  0]
nxt: [-1  2  3  4 -1]
top: [-1 -1  0]
ptr: 0

idea is ptr keeps track where to push. so in push
make arr[ptr] as val, increment ptr as nxt[ptr] nxt will store where
to move there could be scenerios:
We pushed to stk1 [8, 9] and [7] to stk2

arr: [ 8  9  7  0  0]
nxt: [-1  0 -1  4 -1]  After pushing nxt stores top value after pop
top: [-1  2  1]
ptr: 3

If we pop Stk1 then there's a vacancy and new element should be
inserted at that vacant spot. So after pop:

arr: [ 8  9  7  0  0]    [1st elem is garbage val now]
nxt: [-1  3 -1  4 -1]  after puting to vacant 1 nxt is pos 3
top: [-1  2  0]
ptr: 1 */
class kStacks
{
    int k, n;
    vector<int> arr, top, nxt;
    int ptr;

public:
    kStacks(int _k, int _n) : k(_k), n(_n), ptr(0)
    {
        arr.resize(n); top.resize(k, -1); nxt.resize(n);
        for (int i = 0; i < n-1; ++i) nxt[i] = i+1;
        nxt[n-1] = -1;
    }
    inline bool isFull() { return (ptr == -1); }
    inline bool isEmpty(int stk) { return (top[stk] == -1); }
    void push(int x, int stk)
    {
        if (isFull()) return;
        int tp = ptr; ptr = nxt[tp];
        nxt[tp] = top[stk];
        top[stk] = tp;
        arr[tp] = x;
    }
    int pop(int stk)
    {
        if (isEmpty(stk)) return -1;
        int tp = top[stk];
        top[stk] = nxt[tp];
        nxt[tp] = ptr;
        ptr = tp;
        return arr[tp];
    }
};
```

### Stack of Plates

Imagine a literal stack of plates. If the stack gets too high, it might topple. Therefore in real life, we would mimics this. SetOfStacks should be composed of several stacks and should create a new stack once the previous ones exceeds capacity. SetOfStacks.push\(\) and SetOfStacks.pop\(\) should behave identically to a single stack that is, pop\(\) should return the same values as it would if there were just a single stack.

```cpp
const int capacity = 10;
class SetOfStacks
{
    vector<stack<int>> stks;
    void push(int x)
    {
        if (!st.empty() && st.size() != capacity) st.back().push(x);
        else
        {
            stack<int> st(capacity);
            st.push(x);
            stks.push_back(st);
        }
    }
    int pop()
    {
        if (stks.empty()) return -1;
        int res = stks.back().top();
        stks.back().pop();
        if (stks.back().empty()) stks.pop_back();
        return res;
    }
};
```

Follow up: Implement a function popAt\(int index\) which performs a pop operation on a specific sub-stack.

```cpp
/* [1, 2] [3, 4] [5]
Square brackets denotes seperate stacks
let's call pop at 0
[1, 3][4, 5]
This should be output */
int popAt(int i, bool removeTop = true)
{
    int res = (removeTop) ? stk[i].pop() : stk[i].removeBottom();
    if (stk[i].empty()) stk.erase(stk.begin() + i);
    else if (stk.size() > i+1) stk[i].push(popAt(i+1, false));
    return res;
}
// Need to create another function removeBottom in our stack
```

### Queue via Stacks

Implement a MyQueue class which implements a queue using two stacks.

```cpp
class Queue
{
    stack<int> st1, st2;
    int dequeue(int x)    // O(1)
    {
        if (st1.empty()) return -1;
        int res = st1.top(); st1.pop();
        return res;
    }
    void enqueue(int x)    // O(N)
    {
        while (!s1.empty()) st2.push(st1.top()), st1.pop();
        st1.push(x);
        while (!st2.empty()) st1.push(s2.top()), s2.pop();
    }
};

// Using recurssion stack as one stack
class Queue
{
    stack<int> st;
    void enqueue(int x)    // O(1)
    {
        st.push(x);
    }
    int dequeue()        // O(N)
    {
        if (st.empty()) return -1;
        int x = st.top(); st.pop();
        if (st.empty()) return x;
        int res = dequeue();
        st.push(x);
        return res;
    }
};
```

### Stack via Queue

```cpp
class Queue
{
    queue<int> q1, q2;
    int sz = 0;
    int pop()    // O(1)
    {
        if (q1.empty()) return -1;
        int res = q1.front();
        q1.pop(); sz--;
        return res;
    }
    void push(int x)    // O(N)
    {
        q2.push(x); sz++;
        while (!q1.empty()) q2.push(q1.front()), q1.pop();
        swap(q1, q2);
    }
};

class Queue
{
    queue<int> q1, q2;
    int sz = 0;
    void push(int x)    // O(1)
    {
        q1.push(x); sz++;
    }
    int pop()    // O(N)
    {
        if (!q1.empty()) return -1;
        while (q1.size() != 1) q2.push(q1.front()), q1.pop();
        int res = q1.front();
        q1.pop(); sz--;
        swap(q1, q2);
        return res;
    }
};
```

### Sort Stacks

Write a program to sort a stack such that the smallest items are on the top. You can use an additional temporary stack, but you may not copy the elements into any other data structure \(such as an array\). The stack supports the following operations: push, pop, peek, and isEmpty.

```cpp
stack<int> sortStack(stack<int> &x)
{
    stack<int> aux;
    while (!x.empty())
    {
        int cur = x.top(); x.pop();
        while (!aux.empty() && aux.top() > cur)
        {
            x.push(aux.top());
            aux.pop();
        }
        aux.push(x);
    }
}
// O(N^2) : O(N)
```

## Two Pointer method

* Subarray sum problem: We want to find a subarray whose sum is x
* 2SUM problem: given an array of n numbers and a target sum x, find two array values such that their sum is x

### [Minimize the Absolute difference](https://www.interviewbit.com/problems/minimize-the-absolute-difference/)

```cpp
// minimize this | max(a,b,c) - min(a,b,c) |
int Solution::solve(vector<int> &A, vector<int> &B, vector<int> &C)
{
    int i = A.size()-1, j = B.size()-1, k = C.size()-1;
    int ans = INT_MAX;
    while (i >= 0 && j >= 0 && k >= 0)
    {
        int x = max(A[i], max(B[j], C[k]));
        int y = min(A[i], min(B[j], C[k]));
        ans = min(ans, abs(x - y));
        if (A[i] == x) --i;
        else if (B[i] == x) --j;
        else --k;
    }
    if (ans == INT_MAX) return -1;
    else return ans;
}
```

### [Count the Triplets](https://practice.geeksforgeeks.org/problems/count-the-triplets/0)

```cpp
// Count the triplets such that A[i] + A[j] = A[k]
using namespace std;
#define ll long long

int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        vector<int> arr(n);
        for (int i = 0; i < n; ++i) cin >> arr[i];
        sort(arr.begin(), arr.end(), greater<int>());
        int ans = 0;
        for (int i = 0; i < n-2; ++i)
        {
            int j = i+1, k = n-1;
            while (j < k)
            {
                int sum = arr[j] + arr[k];
                if (sum == arr[i]) ++ans;
                if (sum < arr[i]) --k;
                else ++j;
            }
        }
        if (ans == 0) cout << -1 << endl;
        else cout << ans << endl;
    }
    return 0;
}
```

### 3 Sum Closest

```cpp
// Same technique - 3 Sum
int Solution::threeSumClosest(vector<int> &A, int B)
{
    sort(A.begin(), A.end());
    int diff = INT_MAX, ansA, ansB, ansC;
    for (int i = 0; i < A.size()-2; ++i)
    {
        int j = i+1, k = A.size()-1;
        while (j < k)
        {
            int sum = A[i] + A[j] + A[k];
            if (abs(sum - B) < diff) ansA = A[i], ansB = A[j], ansC = A[k];
            if (sum > B) k--;
            else j++;
        }
    }
    return (diff == INT_MAX) ? -1 : ansA + ansB + ansC;
}
```

### [3 Sum \(Without Duplicates\)](https://leetcode.com/problems/3sum/)

```cpp
vector<vector<int>> threeSum(vector<int>& nums)
{
    sort(nums.begin(), nums.end());
    int n = nums.size();
    vector<vector<int>> res;
    int target = 0;
    for (int i = 0; i < n-1; ++i)
    {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        int j = i+1, k = n-1;
        while (j < k)
        {
            int sum = nums[i] + nums[j] + nums[k];
            if (sum == target)
            {
                res.push_back({nums[i], nums[j], nums[k]});
                while (j < k && nums[j] == nums[j+1]) j++;
                while (j < k && nums[k] == nums[k-1]) k--;
                ++j, --k;
            }
            else if (sum > 0) k--;
            else j++;
        }
    }
    return res;
}
```

### [Array 3 Pointers](https://www.interviewbit.com/problems/array-3-pointers/)

```cpp
// Array 3 pointers - max(abs(A[i] - B[j]), abs(B[j] - C[k]), abs(C[k] - A[i])) is minimized
int getMax(int a, int b, int c) { return max(a, max(b,c)); }
int Solution::minimize(const vector<int> &A, const vector<int> &B, const vector<int> &C)
{
    int i = 0, j = 0, k = 0;
    int sol = INT_MAX;
    int temp, temp1, temp2, temp3;
    while(i < A.size() && j < B.size() && k < C.size())
    {
        sol = min(sol, getMax(abs(A[i]-B[j]), abs(B[j]-C[k]), abs(C[k]-A[i])));
        temp1 = (i+1 < A.size()) ?
            getMax(abs(A[i+1]-B[j]), abs(B[j]-C[k]), abs(C[k]-A[i+1])) : INT_MAX;
        temp2 = (j+1 < B.size()) ?
            getMax(abs(A[i]-B[j+1]), abs(B[j+1]-C[k]), abs(C[k]-A[i])) : INT_MAX;
        temp3 = (k+1 < C.size()) ?
            getMax(abs(A[i]-B[j]), abs(B[j]-C[k+1]), abs(C[k+1]-A[i])) : INT_MAX;
        temp = min(temp1, min(temp2, temp3));

        if(temp == INT_MAX) return sol;
        else if(temp == temp1) i++;
        else if(temp == temp2) j++;
        else k++;
    }
    return sol;
}
```

### [DiffK](https://www.interviewbit.com/problems/diffk/)

```cpp
// Diffk - A[i] - A[j] = k, i != j
int Solution::diffPossible(vector<int> &A, int B)
{
    int i = 0, j = 0;
    while (i < A.size() && j < A.size())
    {
        if (i == j) i++;
        int cur = A[i] - A[j];
        if (cur == B) return 1;
        else if (cur > B) j++;
        else i++;
    }
    return 0;
}
```

### Pythagorean Triplet

```cpp
// Pythagorean Triplet - a^2 + b^2 = c^2 satisfy this
using namespace std;
#define ll long long

int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        int arr[n];
        for (int i = 0; i < n; ++i) cin >> arr[i];
        for (int i = 0; i < n; ++i) arr[i] *= arr[i];
        sort(arr, arr + n, greater<int>());
        bool found = false;
        for (int i = 0; i < n-2; ++i)
        {
            int j = i+1, k = n-1;
            while (j < k)
            {
                int b = arr[j] + arr[k];
                if (arr[i] == b)
                {
                    cout << "Yes" << endl;
                    found = true;
                    break;
                }
                else if (arr[i] > b) --k;
                else ++j;
            }
            if (found) break;
        }
        if (!found) cout << "No" << endl;
    }
    return 0;
}

/*
Analysis:
for (int i = 0; i < n-2; ++i)
    int j = i+1, k = n-1;
    while (j < k)
This technique works for Count the triplets such that A[i] + A[j] = A[k],
sum a b c closest to given number and Pythagorean Triplet - a^2 + b^2 = c^2
satisfy this. In these three it worked because this technique is basically
for summation problems. In pythagorean triplet however for optimization
in array every number squared is stored instead to avoid recalculating square.
We do both side pointer technique only if there's an addition problem

minimize this | max(a,b,c) - min(a,b,c) | we start by pointing all i, j, k to n-1.
If max value is contributed by say a then we do i-- if b then j-- else k--

max(abs(A[i] - B[j]), abs(B[j] - C[k]), abs(C[k] - A[i])) is minimized, this
questions starts by pointing i, j, k to 0. It doesn't matter we can point it
to end also. what we are basically checking if we increment i, j and k which
one gives best benefit then we increment that only.

Lastly DiffK we again do linear pointer thing */
```

## Monotonic Queue

### Sliding Window Max/Min:

Key observation: Given an input array A when A\[l\] &lt; A\[r\] for l &lt; r, then A\[l\] should never be returned as the sliding max, if A\[r\] has entered the sliding window.  
So we maintain a monotonic array with index increasing and value decreasing.

```cpp
vector<int> slidingWindowMinMax(vector<int> &arr, int k, bool max = true)
{
    deque<int> q;
    vector<int> res;
    int n = arr.size();
    for (int i = 0; i < n; ++i)
    {
        while (!q.empty() && q.front() < i-k+1) q.pop_front(); // handles index
        if (max)    // handles values
        {
            while (!q.empty() && arr[i] > arr[q.back()])
                q.pop_back();
        }
        else
        {
            while (!q.empty() && arr[i] < arr[q.back()])
                q.pop_back();
        }
        q.push_back(i);
        if(i >= k-1) res.push_back(arr[q.front()]);
    }
    return res;
}
```

> DP problem where `A[i] = max(A[j:k]) + C` for `j < k <= i` can be solved by Monotonic Queue. Example: [https://atcoder.jp/contests/dp/tasks/dp\_b](https://atcoder.jp/contests/dp/tasks/dp_b)

### Find Next Element Larger/Smaller:

Key observation: given `A[k] < A[j] > A[i]` for `k < j < i`, `A[k]` never become the **nearest** element larger than `A[i]` because of `A[j]`.

So we should have a decreasing monotonic queue here. The arrow indicates that the mapping from element on the right to the nearest element on the left larger than it. The elements in the valley are ignored.

![](.gitbook/assets/image%20%28168%29.png)

```cpp
vector<int> nextMonotonicElement(vector<int> &arr, bool greater = true, bool circularArray = false)
{
    vector<int> res(arr.size());
    stack<int> st;
    for (int i = (circularArray) ? (2*arr.size()-1) : (arr.size()-1); i >= 0; --i)
    {
        if (greater)
        {
            while (!st.empty() && arr[st.top()] <= arr[i % arr.size()])
                st.pop();
        }
        else
        {
            while (!st.empty() && arr[st.top()] >= arr[i % arr.size()])
                st.pop();
        }
        res[i % arr.size()] = st.empty() ? -1 : arr[st.top()];
        st.push(i % arr.size());
    }
    return res;
}
```

### [Length of Longest Substring with K distinct characters](https://www.lintcode.com/problem/longest-substring-with-at-most-k-distinct-characters/description)

```cpp
int lengthOfLongestSubstringKDistinct(string &s, int k)
{
    unordered_map<char, int> cnt;
    int res = 0;
    for (int l = 0, r = 0; r < s.size(); ++r)
    {
        cnt[s[r]]++;
        while (cnt.size() > k)
        {
            cnt[s[l]]--;
            if (cnt[s[l]] == 0) cnt.erase(s[l]);
            l++;
        }
        res = max(res, r-l+1);
    }
    return res;
}
```

### [Count Subarrays with product less than K](https://leetcode.com/problems/subarray-product-less-than-k/)

```cpp
// Integer overflow
int numSubarrayProductLessThanK(vector<int>& nums, int k)
{
    if (k == 0) return 0;
    vector<long long> prod(nums.size()+1);
    prod[0] = 1;
    for (int i = 0; i < nums.size(); ++i) prod[i+1] = prod[i] * nums[i];
    int res = 0;
    for (int i = 0; i < nums.size(); ++i)
    {
        auto it = upper_bound(prod.begin(), prod.end(), prod[i+1]/k);
        if (it != prod.end())
            res += (i+1) - (it - prod.begin());
    }
    return res;
}

// Accepted using sliding window
int numSubarrayProductLessThanK(vector<int>& nums, int k)
{
    int res = 0;
    if (k == 0) return 0;
    for (int l = 0, r = 0, curProd = 1; r < nums.size(); ++r)
    {
        curProd *= nums[r];
        while (curProd >= k) { curProd /= nums[l++]; }
        if (l <= r) res += (r-l+1);
    }
    return res;
}
```

### [Largest Area in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

Brute force is to pick two points, find minimum from i to j area within that bound will be min \* \(j-i+1\) just find maximum over all N square i j pairs. Complexity will be N^3 we can optimize it easily to get N^2.

[https://youtu.be/MhQPpAoZbMc](https://youtu.be/MhQPpAoZbMc)

```cpp
int largestRectangleArea(vector<int>& heights)
{
    stack<int> st;
    int ans = 0;
    heights.push_back(-1);
    for (int i = 0; i < heights.size(); ++i)
    {
        while (!st.empty() && heights[i] <= heights[st.top()])
        {
            int height = heights[st.top()];
            st.pop();
            // i is rightmost smaller element, st.top() is
            // prev top leftmost
            int width = i - (st.empty() ? -1 : st.top()) - 1;
            ans = max(ans, height * width);
        }
        st.push(i);
    }
    return ans;
}
```

### [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

Idea: Convert 2D Matrix into 1D height array then task becomes largest rectangle in histogram.

```cpp
int maximalRectangle(vector<vector<char>>& matrix)
{
    if (matrix.empty() || matrix[0].empty()) return 0;
    int n = matrix.size(), m = matrix[0].size();
    vector<int> histogram(m, 0);
    int res = 0;
    for (int i = 0; i < n; ++i)
    {
        for (int j = 0; j < m; ++j)
            histogram[j] = (matrix[i][j] == '1') ? histogram[j]+1 : 0;
        res = max(res, largestRectangleArea(histogram));
    }
    return res;
}
```

### Sort Stack

```cpp
// Make smallest item on top
/*
One approach is to keep finding minimum put it on the stack but
this will require in total of 3 stacks i.e. 2 extra but we can only use 1 extra.
*/

stack<int> addit;
while (!st.empty())
{
    int temp = st.pop();
    while (!addit.empty() && addit.top() > temp) st.push(addit.pop());
    addit.push(temp);
}
while (!addit.empty()) st.push(addit.pop());

// This above one is O(N square)
// If we had infinite stacks then we could have used merge sort tab O(logN)
// hota complexity
```

### Shortest Subarray with Sum at Least K

Return the length of the shortest, non-empty, contiguous subarray of A with sum at least K.

Idea: We want to find A\[l\] A\[l+1\] ... A\[r\] such that its sum\[l..r\] is &gt;= k We can do prefix sum P\[r\] - P\[l-1\] &gt;= k we basically want P\[r\] - k &gt;= P\[l-1\] so upper\_bound will do it. To do it in logN however we need to maintain a monotonic queue.

```cpp
int shortestSubarray(vector<int>& arr, int k, int &r, int &l)
{
    deque<pair<int, int>> dq;
    dq.emplace_back(0, -1);
    int presum = 0, res = INF;
    for (int i = 0; i < arr.size(); ++ i)
    {
        presum += arr[i];
        while(!dq.empty() && presum - dq.front().first >= k)
        {
            int cur = i-dq.front().second;
            if (cur < res) res = cur, r = i, l = i-(cur-1);
            dq.pop_front();
        }
        while(!dq.empty() && dq.back().first >= presum) dq.pop_back();
        dq.emplace_back(presum, i);
    }
    return res == INF ? -1 : res;
}
```

### [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)
```c++
string removeKdigits(string num, int k)
{
    stack<char> st;
    int cnt = k;
    for (char ch : num)
    {
        while (!st.empty() && st.top() > ch && cnt)
        {
            st.pop();
            cnt--;
        }
        st.push(ch);
    }
    string res(st.size(), '0');
    for (int i = st.size()-1; i >= 0; --i)
    {
        res[i] = st.top();
        st.pop();
    }
    int cur = 0, keep = num.size()-k;   // remove leading zeroes from res
    while (cur < keep && res[cur] == '0') ++cur;
    return cur == keep ? "0" : res.substr(cur, keep - cur);
}
```

TODO:

* **LC739. Daily Temperatures**
* **LC901. Online Stock Span**
* **LC907. Sum of Subarray Minimums**

