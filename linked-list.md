# Linked List

### Remove Dups

Write a code to remove duplicates from an unsorted linked list.  
FOLLOW UP: How would you solve this problem if a temporary buffer is not allowed.

```text
Using hashtable we can do in O(N)
Follow up:
We can do brute force O(N^2)
We can sort the array then the consecutive ones will be dups a linear
scan will do. O(NlogN)
```

### Return Kth to Last

Implement an algorithm to find the Kth to last element of a singly linked list.

```text
Take 2 points one at head, other at head+k. Keep incrementing both
until j reaches end. Then i is kth last
```

### Delete Middle Node

Implement an algorithm to delete a node in the middle \(i.e. any node but the first and last node not necessarily the exact middle\) of a singly linked list, given only access to that node.

```cpp
bool deleteNode(Node *n)
{
    if (!n || !(n->next)) return false;
    Node *nxt = n->next;
    n->val = nxt->val;
    n->nxt = nxt->nxt;
    return true;
}
```

### [Partition](https://leetcode.com/problems/partition-list/)

Write code to partition a linked list around a value x, such that all nodes less than x come before all nodes greater than or equal to x. If x is contained within the list, the values of x only need to be after the elements less than x \(see below\). The partition element x can appear anywhere in the "right partition"; it does not need to appear between the left and right partitions.

Input: 3 -&gt; 5 -&gt; 8 -&gt; 5 -&gt; 10 -&gt; 2 -&gt; 1 \[partition = 5\]  
Output: 3 -&gt; 1 -&gt; 2 -&gt;10 -&gt; 5 -&gt; 5 -&gt; 8

```cpp
ListNode* partition(ListNode* head, int x)
{
    ListNode *left = NULL, *right = NULL, *i = NULL, *j = NULL;
    while (head)
    {
        if (head->val < x)
        {
            if (!left) left = head, i = head;
            else i->next = head, i = i->next;
        }
        else
        {
            if (!right) right = head, j = head;
            else j->next = head, j = j->next;
        }
        head = head->next;
    }
    if (j) j->next = NULL;
    if (i) i->next = right;
    return (left) ? left : right;
}
```

### Intersection

Given two single linked lists, determine if the two lists intersect. Return the intersecting node. Note that the intersection is defined based on reference, not value. That is if the kth node of the first linked list is the exact same node \(by reference\) as the jth node of the second linked list then they are intersecting.

```cpp
/* Count length of both the lists. Go exactly |n-m| steps from behind in the
bigger list */
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB)
{
    ListNode *i = headA, *j = headB;
    int n = 0, m = 0;
    while (i) { n++; i = i->next; }
    while (j) { m++; j = j->next; }
    int dif = abs(n - m);
    if (m > n) swap(headA, headB);
    while (dif--) headA = headA->next;
    while (headA != headB)
    {
        headA = headA->next;
        headB = headB->next;
    }
    return headA;
}
```

### Remove nth from end

Kth from last element \(simply ek loop me linked list ka size pata karlo then dusre me n-k pe chale jaao BUT WHAT ARE OTHER WAYS? say size allowed naaho toh simply k element tak jaao then ek aur pointer lo jo head pe ho now dono ko chalao jabktak null naa hojaye pehla wala dusra wala pointer is the ans\)

### Detect if List is Palindrome

Go to the middle of linked list and reverse from middle to the end and break it into two half. Now just iterate both halves for equality.

To go to middle, slow and fast pointer can be used when fast will become NULL \(reach end\) it will cover 2x distance while x distance will be covered by slow pointer which will be then the middle.

### Reverse a Linked List

```cpp
Node *cur = head, *next = NULL, *prev = NULL;
while (cur != NULL)
{
    next = cur->next;
    cur->next = prev;
    prev = cur;
    cur = next;
}
return prev;

// Reverse groups of K
ListNode* reverse(ListNode* first, ListNode* last)
{
    ListNode* prev = last;
    while (first != last)
    {
        auto tmp = first->next;
        first->next = prev;
        prev = first;
        first = tmp;
    }
    return prev;
}
ListNode* reverseKGroup(ListNode* head, int k)
{
    auto node = head;
    for (int i = 0; i < k; ++i)
    {
        if (!node) return head; //nothing to do list too sort
        node = node->next;
    }
    auto new_head = reverse(head, node);
    head->next = reverseKGroup(node, k);
    return new_head;
}

// Reverse within a range
ListNode* Solution::reverseBetween(ListNode* A, int B, int C)
{
    ListNode *head = A, *cur = A, *next = NULL, *prev = NULL;
    int i = 0;
    if (B == 1)
    {
        ListNode *temp = A;
        while (cur != NULL && i < C)
        {
            ++i;
            next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        temp->next = next;
        head = prev;
    }
    else
    {
        while (cur != NULL && i < B-2) ++i, cur = cur->next;
        ListNode *temp = cur;
        cur = cur->next;
        prev = NULL;
        while (cur != NULL && i < C-1)
        {
            ++i;
            next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        temp->next->next = next;
        temp->next = prev;
    }
    return head;
}
```

### Loop

Why Floyd cycle detection Works?: [https://www.youtube.com/watch?v=LUm2ABqAs1w&t=1220s](https://www.youtube.com/watch?v=LUm2ABqAs1w&t=1220s)

```cpp
bool hasCycle(ListNode *head)
{
    if (!head) return false;
    ListNode *slow = head, *fast = head->next;
    while (slow && fast && fast->next)
    {
        slow = slow->next, fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}

void removeTheLoop(Node *A)
{
    Node *slow = A, *fast = A;
    while (slow && fast)
    {
        slow = slow->next;
        if (!fast || !slow || !fast->next) return;
        fast = fast->next->next;
        if (!fast || !slow || !fast->next) return;
        if (slow == fast) break;
    }
    if (!fast || !slow || !fast->next) return;
    fast = A;
    Node* prev = NULL;
    while (slow !=fast)
    {
        prev = slow;
        slow = slow->next;
        fast = fast->next;
    }
    prev->next = NULL;
}
```

### [Copy Linked List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

Given a linked list with next and random pointer create a copy of it.

```cpp
/* One way is to iterate over linked list mapping that node with it's newly
created copy (so that we can later use map to random-access that node).
It's O(N) time with O(N) space */
Node* copyRandomList(Node* head)
{
    unordered_map<Node*, Node*> rec;
    Node *cur = head;
    while (cur)
    {
        rec[cur] = new Node(cur->val);
        cur = cur->next;
    }
    
    cur = head;
    while (cur)
    {
        if (cur->next) rec[cur]->next = rec[cur->next];
        if (cur->random) rec[cur]->random = rec[cur->random];
        cur = cur->next;
    }
    
    return rec[head];
}

/* To do in O(1) space we have to utilize our list only, making it keep all
the relevant informations. What we need is a random access. */
```

![](.gitbook/assets/image%20%28265%29.png)

```cpp
/* We store next to old_node the new_node next of new_node is next_old_node.
Now to random access a node from old_node just go to it's next. */
Node* copyRandomList(Node* head)
{
    Node *cur = head;
    while (cur)
    {
        Node *newNode = new Node(cur->val);
        newNode->next = cur->next;
        cur->next = newNode;
        cur = newNode->next;
    }
    cur = head;
    while (cur)
    {
        if (cur->random) cur->next->random = cur->random->next;
        cur = cur->next->next;
    }
    
    cur = head;
    Node *p, *q, *newList = NULL, *cur2 = NULL;
    while (cur)
    {
        p = cur, q = cur->next->next;
        if (!newList) newList = cur->next, cur2 = cur->next;
        else cur2->next = cur->next, cur2 = cur2->next;
        cur = cur->next->next;
        p->next = q;
    }        
    return newList;
}
```

### [Merge K Sorted Linked Lists](https://www.interviewbit.com/problems/merge-k-sorted-lists/)

```cpp
/*
Interviebit question is bullshit gives correct answer even for high time, in reality this question has tighter constraints and is asked by Google

Way #1: Brute Force - Array me yaa map me saare element daalo sort kardo then linked list banado Time: O(KNlogKN) Space: O(KN)

Way #2: Create K pointers, initially pointing to head of each linked list choose smallest increment it's pointer. Keep doing it. Time: O(KN) Space: O(K). Interview bit variation passes this. One optimization in this is use priority queue to avoid comparisons then time will become: O(NlogK)

Way #3: merge two sorted linked list at a time till (k-1) times. Time: O(KN) Space: O(1)
*/

//Way 1: Using maps insertion will be logN so total time will be: log1 + log2 + .... + log(KN) for insertion this is integration from 1 to n logx = nlogn + n i.e. KNlogKN then inside for loop of map theres KN iteration so overall O(KNLogKN) but in case of duplicacy it stores count so it can perform better in that case
ListNode* Solution::mergeKLists(vector<ListNode*> &A)
{
    map<int, int> myMap;
    for (auto x : A)
        while(x)
            myMap[x->val]++, x = x->next;

    ListNode *head = NULL, *cur = NULL;
    for (auto it = myMap.begin(); it != myMap.end(); ++it)
    {
        while (it->second != 0)
        {
            ListNode *list = new ListNode(it->first);
            if (!head) head = list, cur = list;
            else cur->next = list, cur = cur->next;
            it->second--;
        }
    }
    return head;
}

//Way 2: Just O(K) space, O(KNK) time
ListNode* mergeKLists(vector<ListNode*>& A)
{
    vector<ListNode*> ptrs;
    for (auto x : A) ptrs.push_back(x);
    ListNode *head = NULL, *cur = NULL;
    while (true)
    {
        int minVal = INT_MAX, minIndex = 0;
        for (int i = 0; i < ptrs.size(); ++i)
            if (ptrs[i] && ptrs[i]->val < minVal)
                minVal = ptrs[i]->val, minIndex = i;

        if (minVal == INT_MAX) break;
        ListNode *temp = new ListNode(minVal);
        if (!head) head = temp, cur = head;
        else cur->next = temp, cur = cur->next;
        ptrs[minIndex] = ptrs[minIndex]->next;
    }
    return head;
}

// O(KNlogK) time, O(K) space
typedef pair<int, ListNode*> pilist;
ListNode* Solution::mergeKLists(vector<ListNode*> &A)
{
    priority_queue<pilist, vector<pilist>, greater<pilist> > ptrs;
    for (auto x : A)
        ptrs.push({x->val, x});
    ListNode *head = NULL, *cur = NULL;
    while (!ptrs.empty())
    {
        pilist front = ptrs.top();
        ptrs.pop();
        ListNode *temp = new ListNode(front.first);
        if (!head) head = temp, cur = head;
        else cur->next = temp, cur = cur->next;
        if ((front.second)->next)
        {
            front.second = (front.second)->next;
            front.first = (front.second)->val;
            ptrs.push(front);
        }
    }
    return head;
}
// Conclusion: this one is better but if there are many duplicacy present 1st one is better
```

### [Insert Into Cyclic Sorted List](https://www.lintcode.com/problem/insert-into-a-cyclic-sorted-list/description)
```c++
ListNode * insert(ListNode *head, int x)
{
    if (!head)
    {
        ListNode *node = new ListNode(x);
        node->next = node;
        return node;
    }
    
    ListNode *cur = head, *maxNode = head;
    bool inserted = false;
    while(true)
    {
        if (cur->next->val < cur->val) maxNode = cur;
        if (cur->next->val >= x && cur->val <= x)
        {
            ListNode *node = new ListNode(x, cur->next);
            cur->next = node;
            inserted = true;
            break;
        }
        
        cur = cur->next;
        if (cur == head) break;
    }
    
    // Turning point from max to min
    if (!inserted) maxNode->next = new ListNode(x, maxNode->next);
    return head;
}
```

### Sort Linked List

```cpp
// Merge Sort
ListNode* merge(ListNode* A, ListNode* B)
{
    ListNode *head = NULL, *cur = head, *itA = A, *itB = B;
    while (itA && itB)
    {
        if (itA->val < itB->val)
        {
            if (!head) head = itA, cur = itA;
            else cur->next = itA, cur = cur->next;
            itA = itA->next;
        }
        else
        {
            if (!head) head = itB, cur = itB;
            else cur->next = itB, cur = cur->next;
            itB = itB->next;
        }
    }
    while (itA)
    {
        if (!head) head = itA, cur = itA;
        else cur->next = itA, cur = cur->next;
        itA = itA->next;
    }
    while (itB)
    {
        if (!head) head = itB, cur = itB;
            else cur->next = itB, cur = cur->next;
            itB = itB->next;
    }
    return head;
}

ListNode* sortList(ListNode* head)
{
    if (!head || !head->next) return head;
    ListNode *start = head, *mid = head, *end = head->next;
    while (mid && end && end->next)
    {
        mid = mid->next;
        end = end->next->next;
    }
    ListNode *tmp = mid->next;
    mid->next = NULL;
    mid = tmp;
    return merge(sortList(start), sortList(mid));
}
// Insertion Sort
ListNode* Solution::insertionSortList(ListNode* head)
{
    ListNode *sorted = NULL;
    ListNode *list = head;
    while (list)
    {
        ListNode *curr = list;
        list = list->next;
        if (sorted == NULL || sorted->val > curr->val)
        {
            curr->next = sorted;
            sorted = curr;
        }
        else
        {
            ListNode *tmp = sorted;
            while (tmp)
            {
                ListNode *c = tmp;
                tmp = tmp->next;
                if (c->next == NULL || c->next->val > curr->val)
                {
                    c->next = curr;
                    curr->next = tmp;
                    break;
                }
            }
        }
    }
    return sorted;
}
```

### [Design Skip List](https://leetcode.com/problems/design-skiplist/)

{% embed url="https://youtu.be/2g9OSRKJuzM" %}

We maintain different levels of linkedlist. Let's say we have 2 layers. L1 is the express line and L0 is slower \(having all n\). \|L1\| + \|L0\|/\|L1\| is the worst case analysis, this gives \|L1\|^2 = \|L0\| = n giving size of L1 \(express lane\) as root n for optimal scenario. This 2 layer skip list has root N search, insert and remove complexity.

2 Lanes gives: 2 ²√n  
3 Lanes gives: 3 ³√n  
k Lanes gives: k k√n  
logN Lanes gives: logN logN√n  
simplifying this will give 2 logN

Takes log\(n\) time to add, erase and search

![](.gitbook/assets/image%20%28200%29.png)

Searching is simply start with top most lane, if cur-&gt;right is null or &gt; x then switch to down lane which increments slowly.

To add:

![](.gitbook/assets/image%20%28198%29.png)

blue are all the portion iteration leads us to, before changing lanes we keep track of last relevant pos in that lane in the stack.

We flip a coin after insertion, if tail we move up. Even after finishing all lanes if in the end tail means we create a new tail. So every insertion has a potential of adding a new layer.

```cpp
class Skiplist
{
private:
    struct Node { int val; Node *right, *down; };
    Node *head;

public:
    Skiplist() { head = new Node{0, NULL, NULL}; }
    bool search(int x)
    {
        Node *cur = head;
        while (cur)
        {
            while (cur->right && cur->right->val < x) cur = cur->right;
            if (!cur->right || cur->right->val > x) cur = cur->down;
            else return true;
        }
        return false;
    }
    void add(int x)
    {
        Node *cur = head;
        stack<Node*> st;
        while (cur)
        {
            while (cur->right && cur->right->val < x) cur = cur->right;
            st.push(cur);
            cur = cur->down;
        }

        Node *downNode = NULL;
        bool insertUp = true;
        while (insertUp && !st.empty())
        {
            auto tp = st.top(); st.pop();
            tp->right = new Node{x, tp->right, downNode};
            downNode = tp->right;
            insertUp = (rand()&1);
        }
        if (insertUp)
            head = new Node{0, new Node{x, NULL, downNode}, head};
    }
    bool erase(int x)
    {
        Node *cur = head;
        bool seen = false;
        while (cur)
        {
            while (cur->right && cur->right->val < x) cur = cur->right;
            if (!cur->right || cur->right->val > x) cur = cur->down;
            else
            {
                seen = true;
                cur->right = cur->right->right;
                cur = cur->down;
            }
        }
        return seen;
    }
};
```

