# Graph Theory

## Key Points

* A graph is called dense if it is close to maximum edges otherwise it is sparse. We use adjancy matrix for dense graph and adjancy list for sparse graph. Most of situations are for sparse.
* If not more than 2 children \(then binary tree\) if exactly 0 or 2 children \(then strict binary tree\)
* Indegree of a node is the number of edges that end at the node, and the outdegree of a node is the number of edges that start at the node.
* Full Binary tree: every node has 0 or 2 children
* Complete Binary Tree is a binary tree in which it is completely filled except fot the last level and last level has to be as left as possible so h = log2\(n+1\) - 1
* Perfect Binary Tree: all internal nodes have 2 children and leaves are at the same level.
* A Perfect Binary Tree of height h \(where height is the number of nodes on the path from the root to leaf\) has 2^h – 1 node.
* A graph is bipartite if it is possible to color it's node it using two colors such that no adjacent have same color.
* A graph is simple if no edge starts and ends at the same node. \(self loop, parallel edges\)
* A tree is a connected graph that consists of n nodes and n−1 edges.

![Properties of tree](.gitbook/assets/image%20%28174%29.png)

* To differentiate between tree and graph also we can do Total Nodes = 2Leaves - 1 for tree - Handshaking lemma
* In a k-ary tree where every node has either 0 or k children. L = I\*\(k-1\) + 1

  L = Number of leaf node, I = Number of internal nodes i.e. nodes having k childs

* Inorder traversal of a BST is sorted
* AVL Tree: [https://youtu.be/jDM6\_TnYIqE](https://youtu.be/jDM6_TnYIqE)
* Representations: Adjancy matrix, Adjancy list, Edge list
* **Chromatic Number:** Minimum number of colors required to color Graph G such that no two adjacent vertex gets same color
* **Independent Vertex Set:** Set of vertices in a graph G ,no two of which are adjacent that means no two vertices in this set is connected by an edge
* **Vertex Cover:** Set of Vertices in a Graph G, such that each Edge in G incident on atleast one Vertex in the Set.
* **Edge Cover:** Set of Edges in a Graph G, such that every vertex of G is incident on atleast one of the edges in the set.
* **A clique:** of a graph G is a complete subgraph of G, and the clique of largest possible size is referred to as a _maximum clique_.
* Total number of BSTs possible are catalan\(n\) and Binary Trees possible are catalan\(n\) \* n! **Proof for BST:** cat\(n\) **=** \(2n\)! / \(\(n + 1\)! \* n!\) Consider all possible BST with each element at the root. If there are n nodes, then for each choice of root node, there are n – 1 non-root nodes and these non-root nodes must be partitioned into those that are less than a chosen root and those that are greater than the chosen root.

![](.gitbook/assets/image%20%28203%29.png)

## Traversals

* [https://leetcode.com/problems/binary-tree-inorder-traversal/](https://leetcode.com/problems/binary-tree-inorder-traversal/)
* [https://leetcode.com/problems/binary-tree-preorder-traversal/](https://leetcode.com/problems/binary-tree-preorder-traversal/)
* [https://leetcode.com/problems/binary-tree-postorder-traversal/](https://leetcode.com/problems/binary-tree-postorder-traversal/)

```cpp
vector<int> inorderTraversal(TreeNode* root)
{
    vector<int> res;
    stack<TreeNode*> st;
    auto cur = root;
    while (!st.empty() || cur)
    {
        while (cur) st.push(cur), cur = cur->left;
        cur = st.top(); st.pop();
        res.push_back(cur->val);
        cur = cur->right;
    }
    return res;
}

vector<int> preorderTraversal(TreeNode* root)
{
    vector<int> res;
    if (!root) return res;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty())
    {
        auto cur = st.top(); st.pop();
        res.push_back(cur->val);
        if (cur->right) st.push(cur->right);
        if (cur->left) st.push(cur->left);
    }
    return res;        
}

vector<int> postorderTraversal(TreeNode* root)
{
    vector<int> res;
    if (!root) return res;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty())
    {
        auto cur = st.top(); st.pop();
        res.push_back(cur->val);
        if (cur->left) st.push(cur->left);
        if (cur->right) st.push(cur->right);
    }
    reverse(res.begin(), res.end());
    return res;
}
```

* Moris Traversal - O\(1\) Space Idea is to keep corresponding successor

[https://www.youtube.com/watch?v=wGXB9OWhPTg](https://www.youtube.com/watch?v=wGXB9OWhPTg)

```cpp
/*    10
     /  \
    5    30
   / \    \
  -2  6    40
    \  \
     2  8
    /
   1
Stack basically tells the inorder successor after we are
exhausted with our current subtree. To get rid of stack here

inorder predecessor of 8 is 10 so we will create a link between
8->10

Time complexity is O(N) since we traverse each node atmost 3
times only O(3N) = O(N )*/
vector<int> inorderTraversal(TreeNode* root)
{
    vector<int> res;
    auto cur = root;
    while (cur)
    {
        if (!cur->left)
        {
            res.push_back(cur->val);
            cur = cur->right; // successor link is established
        }
        else
        {
            // tmp is predecessor of cur
            auto tmp = cur->left;
            while (tmp->right && tmp->right != cur)
                tmp = tmp->right;

            // >>> 1 <<< for inorder
            if (!tmp->right)
                tmp->right = cur, cur = cur->left;
            else
            {
                // already traversed in the path so remove link
                tmp->right = NULL;
                res.push_back(cur->val);
                cur = cur->right;
            }
            
            // >>> 2 <<< for preorder
            if (!tmp->right)
            {
                res.push_back(cur->val);
                tmp->right = cur, cur = cur->left;
            }
            else
            {
                tmp->right = NULL;
                cur = cur->right;
            }
        }
    }
    return res;
}

/* >>> 3 <<< for postorder
Same as preorder code except flip left with right */
vector<int> postorderTraversal(TreeNode* root)
{
    vector<int> res;
    auto cur = root;
    while (cur)
    {
        if (!cur->right)
        {
            res.push_back(cur->val);
            cur = cur->left;
        }
        else
        {
            auto tmp = cur->right;
            while (tmp->left && tmp->left != cur)
                tmp = tmp->left;
            
            if (!tmp->left)
            {
                res.push_back(cur->val);
                tmp->left = cur, cur = cur->right;
            }
            else
            {
                tmp->left = NULL;
                cur = cur->left;
            }
        }
    }
    reverse(res.begin(), res.end());
    return res;
}
```

## Binary Tree

### BST Implementation

```cpp
struct Node{ int data; Node *left, *right; } *root = NULL;
Node* searchNode(Node* cur, int x)
{
    if (cur == NULL || cur->data == x) return cur;
    if (cur->data < x) return searchNode(cur->right, x);
    return searchNode(cur->left, x);
}
Node* insertNode(Node* cur, int value)
{
    if (cur == NULL)
    {
        cur->data = value;
        cur->left = NULL;
        cur->right = NULL;
        return cur;
    }
    else if (cur->data > value)
    {
        cur->left = insertNode(cur->left, value);
        return cur;
    }
    else
    {
        cur->right = insertNode(cur->right, value);
        return cur;
    }
}
Node* deleteNode(Node* cur, int value)
{
    if (cur == NULL) return cur;    //If no node with specified value
    else if (cur->data > value)
    {
        cur->left = deleteNode(cur->left, value);
        return cur;
    }
    else if (cur->data < value)
    {
        cur->right = deleteNode(cur->right, value);
        return cur;
    }
    else
    {
        //Case 1 : leaf node
        if (cur->left == NULL && cur->right == NULL)
        {
            delete cur;
            cur = NULL;
            return cur;
        }
        //case 2 : One child
        else if (cur->left == NULL)
        {
            Node *temp = cur;
            cur = cur->right;
            delete temp;
            return cur;
        }
        else if (cur->right == NULL)
        {
            Node *temp = cur;
            cur = cur->left;
            delete temp;
            return cur;
        }
        //Case 3 : Two childrens
        else
        {
            //either find min of right subtree or max of left subtree
            Node* temp = cur->right;
            while(temp->left != NULL) temp = temp->left;
            cur->data = temp->data
            cur->right = deleteNode(cur->right, temp);
            return cur;
        }
    }
}
//O(logN) or in worst case if it's skewed tree then O(N)
```

### Binary Heap Implementation

```cpp
#define MAXSIZE 50
#define parent(x) (x-1) >> 1
#define child1(x) (x << 1) + 1
#define child2(x) (x << 1) + 2

int heapArr[MAXSIZE];
int size = 0;
void push(int x)
{
    if (size == MAXSIZE-1) 
    {
        cout << "DIE!!!" << endl;
        return;
    }
    heapArr[size] = x;
    int cur = size;
    while(heapArr[parent(cur)] > heapArr[cur])
    {
        int temp = heapArr[cur];
        heapArr[cur] = heapArr[parent(cur)];
        heapArr[parent(cur)] = temp;
        cur = parent(cur);
    }
    ++size;
}
void pop()
{
    heapArr[0] = heapArr[size-1];
    --size;
    int cur = 0;
    while(min(child1(cur), child2(cur)) < size)
    {
        int temp = heapArr[cur];
        heapArr[cur] = min(heapArr[child1(cur)], heapArr[child2(cur)]);
        heapArr[min(child1(cur), child2(cur))] = temp;
        cur = min(child1(cur), child2(cur));
    }
}
```

### [Diameter of a Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

```cpp
pair<int, int> DFS(TreeNode* cur)
{
    if (!cur) return {0, 0};
    auto l = DFS(cur->left);
    auto r = DFS(cur->right);
    int diam = max({l.first, r.first, l.second + r.second});
    return {diam, max(l.second, r.second) + 1};
}
int diameterOfBinaryTree(TreeNode* root) { return DFS(root).first; }
```

### Two Sum BST

```cpp
bool Solution::t2Sum(TreeNode* A, int B)
{
    if (!A) return 0;
    stack<TreeNode *> st1, st2;
    TreeNode *cur1 = A, *cur2 = A;
    while(cur1) st1.push(cur1), cur1 = cur1->left;
    while(cur2) st2.push(cur2), cur2 = cur2->right;
    cur1 = st1.top(), cur2 = st2.top();    // cur1 is lowest cur2 is highest
    while (cur1 && cur2 && cur1->val < cur2->val)
    {
        if (cur1->val + cur2->val == B) return true;
        if (cur1->val + cur2->val < B)
        {
            st1.pop();
            cur1 = cur1->right;
            while (cur1) st1.push(cur1), cur1 = cur1->left;
            cur1 = st1.top();
        }
        else
        {
            st2.pop();
            cur2 = cur2->left;
            while (cur2) st2.push(cur2), cur2 = cur2->right;
            cur2 = st2.top();
        }
    }
    return false;
}
```

### Invert Binary Tree

```cpp
void invert(TreeNode* root)
{
    if (root == NULL) return;
    invert(root->left);
    invert(root->right);

    TreeNode* temp = root->left;
    root->left = root->right;
    root->right = temp;
}

TreeNode* Solution::invertTree(TreeNode* root)
{
    invert(root);
    return root;
}
```

### [Flip Equivalent Binary Tree](https://leetcode.com/problems/flip-equivalent-binary-trees/)
```c++
bool flipEquiv(TreeNode* root1, TreeNode* root2)
{
    if (!root1 && !root2) return true;
    if (root1 && root2 && root1->val == root2->val)
    {
        return (flipEquiv(root1->left, root2->left) && flipEquiv(root1->right, root2->right)) ||
            (flipEquiv(root1->left, root2->right) && flipEquiv(root1->right, root2->left));
    }
    return false;
}
```

### [Populate Next Right Pointers in each node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
```c++
Node* connect(Node* root)
{
    if (!root) return NULL;
    queue<Node*> q;
    q.push(root);
    while (!q.empty())
    {
        int sz = q.size();
        for (int i = 0; i < sz; ++i)
        {
            Node *cur = q.front(); q.pop();
            cur->next = (i == sz-1) ? NULL : q.front();
            if (cur->left) q.push(cur->left);
            if (cur->right) q.push(cur->right);
        }
    }
    return root;
}
```

### Symmetric Tree

```cpp
bool check(TreeNode *l, TreeNode *r)
{
    if (!l && !r) return true;
    if (!l || !r) return false;
    if (l->val != r->val) return false;
    return check(l->left, r->right) && check(l->right, r->left);
}
bool isSymmetric(TreeNode* root)
{
    if (!root) return true;
    return check(root->left, root->right);
}
```

### Least Common Ancestor

```cpp
// Naive binary tree implementaiton
TreeNode *LCAinBinaryTree(TreeNode *root, TreeNode *p, TreeNode *q)
{
    int delta = depth(p) - depth(q);
    TreeNode *shallower = (delta > 0) ? q : p;
    TreeNode *deeper = (delta > 0) ? p : q;
    deeper = goUpBy(deeper, abs(delta));
    while (shallower != deeper && !shallower && !deeper)
    {
        shallower = shallower->parent;
        deeper = deeper->parent;
    }
    return (!deeper || !shallower) ? NULL : shallower;
}

// O(N)
TreeNode* LCAinBinaryTree(TreeNode *root, TreeNode *p, TreeNode *q)
{
    if (!root || root->val == p->val || root->val == q->val) return root;
    auto L = LCA(root->left, p, q);
    auto R = LCA(root->right, p, q);
    if (L && R) return root;
    else return L ? L : R;
}
// O(logN) worst case still O(N) in linkedlist type BST
TreeNode* LCAinBST(TreeNode *root, TreeNode *p, TreeNode *q)
{
    // If both p and q are greater than parent then look at right side
    if (root->val < p->val && root->val < q->val)
        return LCA(root->right, p, q);
    // If both p and q are lower than parent then look at left side
    else if (root->val > p->val && root->val > q->val)
        return LCA(root->left, p, q);
    else
        return root;
}
```

### [Verify Preorder Sequence in BST](https://www.lintcode.com/problem/verify-preorder-sequence-in-binary-search-tree/description)

```cpp
/* Consider this testcase: [4,3,5,1,2,3] it should give false
    4
   / \
  3   5
     /
     1
     \
      2
       \
        3
 Subtree 5 should contain all nodes greater than 4 to be valid */
bool verifyPreorder(vector<int> &preorder)
{
    stack<int> st;
    int otherHalfMax = INT_MIN;
    for (auto &x : preorder)
    {
        if (x < otherHalfMax) return false;
        while (!st.empty() && x > st.top())
        {
            otherHalfMax = st.top();
            st.pop();
        }
        st.push(x);
    }
    return true;
}
/* When we are at a node, all of it's subtree (coming vals in array) must be
smaller then parent. stack store exactly the left half so for above example
[4 3    otherHalfMax finds max among these groups
[5 1
[2
[3 */
```

### Construct Binary Tree

To construct a binary tree we need either

**Preorder + Inorder:**  
ABCDEFCGHJLK, DBFEAGCLJHK

In preorder left most is the root node. \(A\)  
Then see it in Inorder \(DBFE\) left - \(GCLJHK\) right

![](.gitbook/assets/image%20%28133%29.png)

Again recursively check left most of DBFE then see it in Inorder. to solve the entire tree.

**Postorder + Inorder:**  
Here everything will be simillar as 1\) except the rightmost is the root node.

**Preorder + Postorder:**  
ABDGHKCEF, GKHDBEFCA A is root node, B is left to A  
B in post order has B-EFC-A so B is left EFC is right to A

![](.gitbook/assets/image%20%28134%29.png)

```cpp
/* Minimal Tree
Given a sorted (increasing order) array with unique integer
elements, write an algorithm to create a binary search tree with
minimal height. */
TreeNode* makeTree(vector<int> &A, int start, int end)
{
    if (start > end) return NULL;
    int mid = (start + end) / 2;
    TreeNode* root = new TreeNode(A[mid]);
    root->left = makeTree(arr, start, mid-1);
    root->right = makeTree(arr, mid+1, end);
    return root;
}
TreeNode* Solution::buildTree(vector<int> &A)
{
    return makeTree(A, 0, A.size()-1);
}

// Construct maximum tree
TreeNode* makeTree(vector<int> &A, int start, int end)
{
    if (start > end) return NULL;
    int maxIndex = max_element(A.begin()+start, A.begin()+end+1) - A.begin();
    TreeNode* root = new TreeNode(A[maxIndex]);
    root->left = makeTree(A, start, maxIndex-1);
    root->right = makeTree(A, maxIndex+1, end);
    return root;
}
TreeNode* Solution::buildTree(vector<int> &A)
{
    return makeTree(A, 0, A.size()-1);
}
​
// Construct using Inorder and Preorder
// for postorder and inorder follow comments
class Solution {
public:
    /*
    preorder [3 8 20 15 7]      inorder [9 3 15 20 7]
                    3
                   / \
                  9   20
                     /  \
                    15   7
    */
    unordered_map<int, int> indexes;
    TreeNode* makeTree(vector<int> preorder, vector<int> inorder, int start, int end)
    {
        if (start > end) return NULL;
        int parentIndex = INT_MAX, otherIndex;    // change INT_MAX to INT_MIN
        for (int i = start; i <= end; ++i)
        {
            if (indexes[inorder[i]] < parentIndex)    // change sign
                parentIndex = indexes[inorder[i]], otherIndex = i;
        }
            
        TreeNode *parent = new TreeNode(preorder[parentIndex]);
        parent->left = makeTree(preorder, inorder, start, otherIndex-1);
        parent->right = makeTree(preorder, inorder, otherIndex+1, end);
        return parent;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder)
    {
        indexes.clear();
        for (int i = 0; i < preorder.size(); ++i) indexes[preorder[i]] = i;
        return makeTree(preorder, inorder, 0, preorder.size()-1);
    }
};
​
/* Construct from preorder and postorder
      1
     / \
    2   3
   / \
   4 5            preorder: 1 2 4 5 3    postorder: 4 5 2 3 1

We can pick the top most from preorder, that's root node
it may or may not have left-right subtree. We can maintain
a stack of levels so here 1->2->4 but now we have to realize
when to stop. So it's clear that here it is post[0] since its
left most. if we keep iterating over post we can maintain it */
class Solution {
public:
    TreeNode* constructFromPrePost(vector<int>& pre, vector<int>& post)
    {
        if (pre.empty()) return NULL;
        stack<TreeNode*> st;
        st.push(new TreeNode(pre[0]));
        // i is in pre, j is in post
        for (int i = 1, j = 0; i < pre.size(); ++i)
        {
            TreeNode *cur = new TreeNode(pre[i]);
            while (st.top()->val == post[j]) st.pop(), j++;
            if (!st.top()->left) st.top()->left = cur;
            else st.top()->right = cur;
            st.push(cur);
        }
        while (st.size() > 1) st.pop();
        return st.top();
    }
};

// Construct BST from sorted linked list
// O(NlogN) solution
ListNode* findMiddle(ListNode* head)
{
    ListNode *prev = NULL, *slow = head, *fast = head;
    while (slow && fast && fast->next)
    {
        prev = slow;
        slow = slow->next;
        fast = fast->next->next;
    }
    if (prev) prev->next = NULL;
    return slow;
}
TreeNode* sortedListToBST(ListNode* head)
{
    if (!head) return NULL;

    ListNode *mid = findMiddle(head);
    TreeNode *root = new TreeNode(mid->val);
    if (head == mid) return root;
    root->left = sortedListToBST(head);
    root->right = sortedListToBST(mid->next);
    return root;
}

// O(N) solution: Form array out of linked list or best use something like Skip List
​
// Construct BST from preorder
TreeNode* construct(vector<int> &preorder, int l, int r)
{
    if (l > r) return NULL;
​
    int leftStart = l+1, rightStart = l;
    while (rightStart < preorder.size() && preorder[rightStart] <= preorder[l])
        rightStart++;
    TreeNode* root = new TreeNode(preorder[l]);
    root->left = construct(preorder, leftStart, rightStart-1);
    root->right = construct(preorder, rightStart, r);
    return root;
}
TreeNode* bstFromPreorder(vector<int>& preorder)
{
    if (preorder.empty()) return NULL;
    return construct(preorder, 0, preorder.size()-1);
}
​
// Recover tree from preorder
// "1-401--349---90--88"    =   [1,401,null,349,88,90]
TreeNode* recoverFromPreorder(string S)
{
    stack<TreeNode*> st;
    for (int p = 0, len = 0, level = 0; p < S.size(); p += len)
    {
        level = 0, len = 0;
        while (S[p] == '-') ++level, ++p;
        while (p + len < S.size() && S[p + len] != '-') ++len;
        TreeNode *n = new TreeNode(stoi(S.substr(p, len)));
        while (st.size() > level) st.pop();
        if (!st.empty())
        {
            if (!st.top()->left) st.top()->left = n;
            else st.top()->right = n;
        }
        st.push(n);
    }
    while (st.size() > 1) st.pop();
    return st.top();
}
```

### [Binary Tree Longest Consecutive Sequence](https://www.lintcode.com/problem/binary-tree-longest-consecutive-sequence/description)
```c++
unordered_map<TreeNode*, int> dp;
void solve(TreeNode* root, TreeNode* par = NULL)
{
    if (!root) return;
    dp[root] = (par && root->val == par->val+1) ? dp[par]+1 : 1;
    solve(root->left, root);
    solve(root->right, root);
}
int longestConsecutive(TreeNode* root)
{
    dp.clear();
    solve(root);
    int res = 0;
    for (auto &x : dp) res = max(res, x.second);
    return res;
}
```

### [Flatten BST to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

```cpp
/*
    1                1
   / \                2
  2   5        ->      3
 / \                    4
3  4                     5

To do it in-place we cannot begin with setting 1 then 2 then 3
we have to go from bottom. */
void flatten(TreeNode* root)
{
    TreeNode *cur = root;
    while (cur)
    {
        if (cur->left)
        {
            TreeNode *tmp = cur->left;
            while (tmp->right) tmp = tmp->right;
            tmp->right = cur->right;
            cur->right = cur->left;
            cur->left = NULL;
        }
        cur = cur->right;
    }
}
```

### [Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

```cpp
int maxPathSum(TreeNode* root)
{
    int ans = INT_MIN;
    DFS(root, ans);
    return ans;
}
int DFS(TreeNode* root, int &ans)
{
    if (!root) return 0;
    int left = max(0, DFS(root->left, ans));
    int right = max(0, DFS(root->right, ans));
    ans = max(ans, left + right + root->val);
    return max(left, right) + root->val;
}
```

### [Generate All Unique BST](https://leetcode.com/problems/unique-binary-search-trees-ii/)

```cpp
/* Generate all unique BSTs
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3 */
// Generate total cat(n) BSTs
class Solution {
public:
    vector<TreeNode*> genTree(int start, int end)
    {
        vector<TreeNode*> res;
        if (start >= end)
        {
            if (start == end) res.push_back(new TreeNode(start));
            else res.push_back(NULL);
            return res;
        }
        vector<TreeNode*> left, right;
        for (int i = start; i <= end; ++i)
        {
            left = genTree(start, i-1);
            right = genTree(i+1, end);
            for (auto x : left)
            {
                for (auto y : right)
                {
                    TreeNode *cur = new TreeNode(i);
                    cur->left = x, cur->right = y;
                    res.push_back(cur);
                }
            }
        }
        return res;
    }
    vector<TreeNode*> generateTrees(int n)
    {
        if (n == 0) return {};
        return genTree(1, n);
    }
};
```

### [Validate BST / Check BST](https://leetcode.com/problems/validate-binary-search-tree/)

```cpp
/* Validate BST
The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees. */
class Solution {
public:
    bool check(TreeNode *root, TreeNode *lower, TreeNode *higher)
    {
        if (!root) return true;
        if (lower && root->val <= lower->val) return false;
        if (higher && root->val >= higher->val) return false;
        return check(root->left, lower, root) && check(root->right, root, higher);
    }
    bool isValidBST(TreeNode* root)
    {
        return check(root, NULL, NULL);
    }
};
```

### [Count Complete Tree Node](https://leetcode.com/problems/count-complete-tree-nodes/)

```cpp
/* Count Complete Tree Nodes
                1
               / \
              2   3
             / \  /
            4  5 6
        output = 6
*/
int countNodes(TreeNode* root)
{
    if (!root) return 0;
    TreeNode *l = root, *r = root;
    int heightL = 0, heightR = 0;
    while (l) heightL++, l = l->left;
    while (r) heightR++, r = r->right;
    if (heightL == heightR) return (1<<heightL)-1;
    return 1 + countNodes(root->left) + countNodes(root->right);
}
```

### [Complete Binary Tree Inserter](https://leetcode.com/problems/complete-binary-tree-inserter/description/)

```cpp
class CBTInserter {
public:
    vector<TreeNode*> tree;     // store tree nodes to a list in bfs order
    CBTInserter(TreeNode* root)
    {
        tree.push_back(root);
        for (int i = 0; i < tree.size();++i)
        {
            if (tree[i]->left) tree.push_back(tree[i]->left);
            if (tree[i]->right) tree.push_back(tree[i]->right);
        }
    }
    int insert(int v)       // insert node to very last level (create one) find it's parent
    {
        int N = tree.size();
        TreeNode* node = new TreeNode(v);
        tree.push_back(node);
        if (N&1) tree[(N - 1) / 2]->left = node;
        else tree[(N - 1) / 2]->right = node;
        return tree[(N - 1) / 2]->val;
    }
    TreeNode* get_root() { return tree[0]; }
};
```

### [BST Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

```cpp
stack<TreeNode*> st;
void pushAll(TreeNode *root)
{
    while (root)
    {
        st.push(root);
        root = root->left;
    }
}
BSTIterator(TreeNode* root) { pushAll(root); }
bool hasNext() { return !st.empty(); }
int next()
{
    auto cur = st.top(); st.pop();
    pushAll(cur->right);
    return cur->val;
}
```

### [Recover BST](https://www.interviewbit.com/problems/recover-binary-search-tree/#)

Two elements of BST are swapped by mistake, find those 2 points.

```cpp
/* In inorder traversal previous value must not be greater
 then cur */
vector<int> Solution::recoverTree(TreeNode* A)
{
    stack<TreeNode*> st;
    TreeNode *cur = A, *prev = NULL, *first = NULL, *last = NULL;
    while(!st.empty() || cur)
    {
        while (cur) st.push(cur), cur = cur->left;
        cur = st.top(); st.pop();
        if (prev)
        {
            if (prev->val > cur->val)
            {
                /* This part is tricky we could have just
                written first = prev, last = cur and then
                break BUT agar non immediate child se swap
                hua he tab uss case me ek toh humesha fixed
                rahega dusra badalta rahega */
                if (!first) last = prev;
                first = cur;
            }
        }
        prev = cur;
        cur = cur->right;
    }
    vector<int> sol;
    if (first && last)
    {
        sol.push_back(first->val);
        sol.push_back(last->val);
    }
    sort(sol.begin(), sol.end());
    return sol;
}
```

### [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

```cpp
// Serialize & Deserialize Binary Tree
// If BST then very simple, store preorder value but here it's a general binary tree
// We can have inorder and preorder, postorder and inorder, or this trick below
class Codec {
private:
    /*
            1
           / \
          2   3
             / \
            4   5
        1[2,3[4,5]]		preorder traverse put [ when a new layer
        							comes in    */
    void encode(TreeNode *root, string &res)
    {
        if (!root) return;
        res += to_string(root->val);
        if (root->left || root->right)
        {
            res += '[';
            encode(root->left, res);
            res += ',';
            encode(root->right, res);
            res += ']';
        }
    }
    TreeNode *decode(string &data, int &pos)
    {
        int i; bool foundL = false;
        for (i = pos; i < data.size(); ++i)
        {
            if (data[i] == ',' || data[i] == ']') break;
            if (data[i] == '[') { foundL = true; break; }
        }

        if (i == pos) return NULL;
        int val = stoi(data.substr(pos, i-pos));
        auto node = new TreeNode(val);
        if (i == data.size()) return node;

        pos = i;
        if (foundL)
        {
            pos++;  // skip '['
            node->left = decode(data, pos);
            pos++;  // skip ','
            node->right = decode(data, pos);
            pos++;  // skip ']'
        }
        return node;
    }

public:
    string serialize(TreeNode* root)
    {
        string res = "";
        encode(root, res);
        return res;
    }

    TreeNode* deserialize(string data)
    {
        if (data == "") return NULL;
        int pos = 0;
        return decode(data, pos);
    }
};
```

* [https://www.lintcode.com/problem/path-sum-iv/](https://www.lintcode.com/problem/path-sum-iv/)

```cpp
/* In this problem, binary tree with depth < 5 is serialized
as a list of 3 digit integers.
eg: [113, 215, 221] -> means first node (hunread 1 means 1
depth, tense 1 means horizontal pos, once 3 means val)
    3
   / \
  5   1
We are given such list and our goal is to deserialize it and
find all path sum from root to child. */

unordered_map<int, int> rec[6];
// ^^ 6 instead of 5 to avoid segmentation fault due to dep+1
void dfs(vector<int> &nums, int &res, int dep = 1, int pos = 1, int sum = 0)
{
    sum += rec[dep][pos];
    if (rec[dep+1].find(2*pos - 1) == rec[dep+1].end() && rec[dep+1].find(2*pos) == rec[dep+1].end()) res += sum;

    if (rec[dep+1].find(2*pos - 1) != rec[dep+1].end()) dfs(nums, res, dep+1, 2*pos - 1, sum);
    if (rec[dep+1].find(2*pos) != rec[dep+1].end()) dfs(nums, res, dep+1, 2*pos, sum);
}
int pathSumIV(vector<int> &nums)
{
    if (nums.empty()) return 0;
    for (auto &x : rec) x.clear();
    for (auto &x : nums)
    {
        int dep = x/100, pos = (x%100)/10, val = x%10;
        rec[dep][pos] = val;
    }
    int res = 0;
    dfs(nums, res);
    return res;
}
```

### Path with Given Sum

* [https://leetcode.com/problems/path-sum](https://leetcode.com/problems/path-sum)
* [https://leetcode.com/problems/path-sum-ii](https://leetcode.com/problems/path-sum-ii)
* [https://leetcode.com/problems/path-sum-iii](https://leetcode.com/problems/path-sum-iii)

```cpp
// root to leaf path sum = sum, check
bool hasPathSum(TreeNode* root, int sum)
{
    if (!root) return false;
    int val = sum - root->val;
    if (val == 0 && !root->left && !root->right) return true;
    return hasPathSum(root->left, val) || hasPathSum(root->right, val);
}

// find all root to leaf paths of given sum
void dfs(TreeNode* root, int sum, vector<int> &cur, vector<vector<int>> &res)
{
    int val = sum - root->val;
    if (!root->left && !root->right && val == 0)
    {
        res.push_back(cur);
        return;
    }
    if (root->left) { cur.push_back(root->left->val); dfs(root->left, val, cur, res); cur.pop_back(); }
    if (root->right) { cur.push_back(root->right->val); dfs(root->right, val, cur, res); cur.pop_back(); }
}
vector<vector<int>> pathSum(TreeNode* root, int sum)
{
    vector<vector<int>> res;
    if (!root) return res;
    vector<int> cur;
    cur.push_back(root->val);
    dfs(root, sum, cur, res);
    return res;
}

/* What if we want all the paths (not just ending at leaf)
but can start and end anywhere
          10                        10
         /  \                      /  \
        5   -3                    15   7
       / \    \                  /  \   \
      3   2   11                18  17  18
     / \   \                   / \    \
    3  -2   1                 21 16   18

idea is simmilar to how we do this kind of problem using
hashing and prefix sum in an array.
we will maintain a prefix sum while doing dfs then use
also maintaing previous prefsums in a hashmap then we
just have to add count of sum-target from hashmap */
unordered_map<int, int> cnt;
void dfs(TreeNode *root, int &target, int &res, int sum = 0)
{
    if (!root) return;
    sum += root->val;
    res += cnt[sum - target];
    cnt[sum]++;
    dfs(root->left, target, res, sum);
    dfs(root->right, target, res, sum);
    cnt[sum]--;
}
int pathSum(TreeNode* root, int sum)
{
    cnt.clear();
    int res = 0;
    cnt[0]++;
    dfs(root, sum, res);
    return res;
}
```

### [Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

```cpp
/* Idea is once we find node with matching key we will swap
it with its successor which is righ to it then all the way left
if right doesnt exist then simply swap it with left. */
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key)
    {
        if (!root) return NULL;
        if (root->val == key)
        {
            if (!root->right)
            {
                auto left = root->left;
                delete root;
                return left;
            }
            else
            {
                auto succ = root->right;
                while (succ->left) succ = succ->left;
                swap(root->val, succ->val);
                // having delete succ will cause runtime error
            }
        }
        root->left = deleteNode(root->left, key);
        root->right = deleteNode(root->right, key);
        return root;
    }
};
```

### [Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)

- (Same) https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/
- every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

```cpp
/*
Convert BST to greater tree
Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13

Output: The root of a Greater Tree like this:
             18
            /   \
          20     13
This follows a particular traversal, if taken that path we only have to sum up
numbers */
int sum = 0;
TreeNode* convertBST(TreeNode* root)
{
    if (!root) return NULL;
    convertBST(root->right);
    sum += root->val;
    root->val = sum;
    convertBST(root->left);
    return root;
}
```

### [Duplicates in Subtree](https://leetcode.com/problems/find-duplicate-subtrees/)

```cpp
/*
Duplicates in sub tree
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4

    2       4       are two duplicates
   /
  4
return [[2,4], [4]]
*/
class Solution {
public:
    vector<TreeNode*> res;
    unordered_map<string, bool> rec;
    string serialize(TreeNode *root)
    {
        if (!root) return "";
        string cur = "[" + serialize(root->left) + to_string(root->val) + serialize(root->right) + "]";
        if (rec.find(cur) != rec.end())
        {
            if (!rec[cur])
            {
                res.push_back(root);
                // being true means we have handled that duplicate so no need to handle it later
                rec[cur] = true;
            }
        }
        else rec[cur] = false;
        return cur;
    }
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root)
    {
        rec.clear(); res.clear();
        serialize(root);
        return res;
    }
};
```

### [Trim BST](https://leetcode.com/problems/trim-a-binary-search-tree/)

```cpp
// Trim BST, remove those which are out of L & R range
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int L, int R)
    {
        if (!root) return NULL;
        if (root->val > R) return trimBST(root->left, L, R);
        if (root->val < L) return trimBST(root->right, L, R);
        root->left = trimBST(root->left, L, R);
        root->right = trimBST(root->right, L, R);
        return root;
    }
};
```

### [Longest Univalent Path](https://leetcode.com/problems/longest-univalue-path/)

Find longest path \(between any 2 node of a tree\) having same val

```cpp
// it returns max depth matching with val
// it makes ans of this subtree
int solve(TreeNode* root, int val, int &ans)
{
    if (!root) return 0;
    int resL = 0, resR = 0;
    int depL = solve(root->left, root->val, resL);
    int depR = solve(root->right, root->val, resR);
    ans = max({depL+depR, resL, resR});
    return (root->val == val) ? max(depL, depR)+1 : 0;
}
int longestUnivaluePath(TreeNode* root)
{
    if (!root) return 0;
    int ans = 0;
    solve(root, -1, ans);
    return ans;
}
```

### [All Possible Full Binary Tree](https://leetcode.com/problems/all-possible-full-binary-trees/)

```cpp
unordered_map<int, vector<TreeNode*>> cache;
vector<TreeNode*> allPossibleFBT(int N)
{
    vector<TreeNode*> res;
    if (cache.find(N) != cache.end()) return cache[N];
    if (N == 1) res.push_back(new TreeNode(0));
    else
    {
        for (int l = 1; l < N; l += 2)
        {
            int r = N-1-l;
            for (auto L : allPossibleFBT(l))
            {
                for (auto R : allPossibleFBT(r))
                {
                    auto root = new TreeNode(0);
                    root->left = L, root->right = R;
                    res.push_back(root);
                }
            }
        }
    }
    return cache[N] = res;
}
```

### [Insert Into BST](https://leetcode.com/problems/insert-into-a-binary-search-tree)

```cpp
TreeNode* insertIntoBST(TreeNode* root, int val)
{
    if (!root) return new TreeNode(val);
    TreeNode *cur = root, *prev = NULL;
    while (cur)
    {
        prev = cur;
        cur = (val < cur->val) ? cur->left : cur->right;
    }
    if (val < prev->val) prev->left = new TreeNode(val);
    if (val > prev->val) prev->right = new TreeNode(val);
    return root;
}
```

### [Smallest String Starting from Leaf](https://leetcode.com/problems/smallest-string-starting-from-leaf/)

```cpp
void dfs(TreeNode *root, string &ans, string str = "")
{
    str = string(1, 'a' + root->val) + str;
    if (root->left) dfs(root->left, ans, str);
    if (root->right) dfs(root->right, ans, str);
    if (!root->left && !root->right)
    {
        if (ans.empty()) ans = str;
        else ans = min(ans, str);
    }
}
string smallestFromLeaf(TreeNode* root)
{
    string ans = "";
    if (!root) return ans;
    dfs(root, ans);
    return ans;
}
```

### [Maximum Difference Between Node & Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

```cpp
int maxAncestorDiff(TreeNode* root, int minV = INT_MAX, int maxV = INT_MIN)
{
    if (!root) return maxV - minV;
    maxV = max(maxV, root->val);
    minV = min(minV, root->val);
    return max(maxAncestorDiff(root->left, minV, maxV),
        maxAncestorDiff(root->right, minV, maxV));
}
```

### [Binary Tree Coloring Game](https://leetcode.com/problems/binary-tree-coloring-game/)

```cpp
class Solution {
public:
    /*
        First player will choose X node next second player
        will need to choose some Y such that
        it should give better connectivity then X if yes
        then second wins otherwise first wins.
        Both wont have same points because nodes are odd
                 1
               /   \
              2     3
             / \    /\
            4   5   6 7
           /\  /\
          8 9 10 11
          X is given 3, we can choose Y as 2, 6, 7...
          With
          (Y=2) 1st player:4 2nd player:7 so 2nd player
          will win return true
    */
    TreeNode* find(TreeNode* root, int x)
    {
        if (!root) return NULL;
        if (root->val == x) return root;
        auto l = find(root->left, x);
        auto r = find(root->right, x);
        return (l ? l : r);
    }
    int count(TreeNode *root)
    {
        if (!root) return 0;
        return count(root->left) + count(root->right) + 1;
    }
    bool btreeGameWinningMove(TreeNode* root, int n, int x)
    {
        auto node = find(root, x);
        int l = count(node->left);
        int r = count(node->right);
        int par = (node != root) ? (n-(l+r+1)) : 0;
        int blueConnectivity = max({l, r, par});
        int redConnectivity = n - blueConnectivity;
        return blueConnectivity > redConnectivity;
    }
};
```

### [Upside Down Binary Tree](https://www.lintcode.com/problem/binary-tree-upside-down/description)

```cpp
/*  1
   / \
  2   3
 / \
4   5
and the output is
   4
  / \
 5   2
    / \
   3   1
Given a binary tree where all the right nodes are either
leaf nodes with a sibling, or empty*/
TreeNode * upsideDownBinaryTree(TreeNode * root)
{
    if (!root) return root;
    TreeNode *cur = root;
    vector<TreeNode*> list;
    while (cur) list.push_back(cur), cur = cur->left;
    TreeNode* res = list.back();
    list.pop_back();
    while (!list.empty())
    {
        cur = list.back();
        list.pop_back();
        cur->left->left = cur->right;
        cur->left->right = cur;
        cur->left = NULL;
        cur->right = NULL;
    }
    return res;
}
```

### [Clone Graph / Duplicate Graph](https://leetcode.com/problems/clone-graph/)

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;

    Node() {}

    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/
unordered_map<int, Node*> cache;
Node* solve(Node* node)
{
    if (!node) return NULL;
    if (cache.find(node->val) != cache.end()) return cache[node->val];
    vector<Node*> newNeigh;
    cache[node->val] = new Node(node->val, newNeigh);
    for (auto &x : node->neighbors)
    {
        auto cur = solve(x);
        if (cur) cache[node->val]->neighbors.push_back(cur);
    }
    return cache[node->val];
}
Node* cloneGraph(Node* node)
{
    cache.clear();
    return solve(node);
}
```

### [Cousins in Binary Tree](https://leetcode.com/problems/cousins-in-binary-tree/)
```c++
void findNode(TreeNode* root, int depth, const int target, pair<int, TreeNode*> &toFind,
                  TreeNode *par = NULL)
{
    if (!root) return;
    if (root->val == target)
    {
        toFind = {depth, par};
        return;
    }
    findNode(root->left, depth+1, target, toFind, root);
    findNode(root->right, depth+1, target, toFind, root);
}
bool isCousins(TreeNode* root, int x, int y)
{
    pair<int, TreeNode*> X, Y;
    findNode(root, 0, x, X);
    findNode(root, 0, y, Y);
    return (X.first == Y.first && X.second != Y.second);
}
```

### [Binary Tree Pruning](https://leetcode.com/problems/binary-tree-pruning/)

Remove every subtree not containing a 1 in it

```cpp
TreeNode* pruneTree(TreeNode* root)
{
    if (!root) return NULL;
    root->left = pruneTree(root->left);
    root->right = pruneTree(root->right);
    bool containsOnes = (root->val == 1 || root->left || root->right);
    if (!containsOnes)
    {
        delete root;
        return NULL;
    }
    return root;
}
```

### Check Subtree

T1 and T2 are two very large binary trees, with T1 much bigger than T2, Create an algorithm to determine if T2 is a subtree of T1.  
A tree T2 is a subtree of T1 if there exists a node n in T1 such that the subtree of n is identical to T2. That is, if you cut off the tree at node n, the two trees would be identical.

```cpp
bool isSubtree(TreeNode* s, TreeNode* t)
{
    if (!s) return false;
    return isSameTree(s, t) || isSubtree(s->left, t) || isSubtree(s->right, t);
}
bool isSameTree(TreeNode *s, TreeNode *t)
{
    if (!s && !t) return true;
    if (!s || !t) return false;
    if (s->val == t->val) return isSameTree(s->left, t->left) && isSameTree(s->right, t->right);
    else return false;
}
```

### Random Node

You are implementing a binary tree class from scratch which, in addition to insert, find, and delete, has a method getRandomNode\(\) which returns a random node from the tree. All nodes should be equally likely to be chosen. Design and implement an algorithm for getRandomNode, and explain how you would implement the rest of the methods.

```cpp
/* There's an stl random function which generates uniform random number.
To implement it, however we will need to use reservoir sampling */

int res;    // our rand node
int getRandomNode() { return (tree.empty()) ? -1 : res; }
void insert(int x)
{
    // Insert x in tree
    if (rand()%len == 0) res = x;
    len++;
}
void remove(int x)
{
    // Delete x in tree
    /* Reservoir sampling cannot do removal. So make getRandomNode O(N)
    Removal is supported there are some research papers on it. */
}
```

## BFS DFS

### Connectivity Check

We can check if a graph is connected by starting at an arbitrary node and finding out if we can reach all other nodes

### Finding Cycle

* A graph contains a cycle if during a graph traversal, we find a node whose neighbour \(other than the previous-parent node in the current path\) has already been visited.
* Another way is to simply calculate the number of nodes and edges in every components. For a component to not contain cycle it must have x nodes and x-1 edges.

### [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)
We need to find those node which don't form cycle in a given directed graph
```c++
const uint8_t UNVISITED = 0, VISITING = 1, VISITED = 2;
bool dfs(vector<vector<int>>& graph, vector<uint8_t> &dp, int u)
{
    dp[u] = VISITING;
    for (const int v : graph[u])
    {
        if (dp[v] == VISITING) return false;
        if (dp[v] == UNVISITED)
            if (!dfs(graph, dp, v)) return false;
    }
    dp[u] = VISITED;
    return true;
}
vector<int> eventualSafeNodes(vector<vector<int>>& graph)
{
    vector<uint8_t> dp(graph.size(), UNVISITED);
    vector<int> res;
    for (size_t i = 0; i < graph.size(); ++i)
    {
        if (dp[i] == UNVISITED) dfs(graph, dp, i);
        if (dp[i] == VISITED) res.push_back(i);
    }
    return res;
}
```

### Bipartiteness check

The idea is to colour the starting node blue, all its neighbours red, all their neighbours blue, and so on. If at some point of the search we notice that two adjacent nodes have the same colour, this means that the graph is not bipartite.

This algorithm of graph coloring works because of only 2 colors \(choosing whether to paint root as blue or red won't matter\). In general case finding graph coloring is np complete and finding minimum colors reqd is np hard.

### Finding Bridges
- https://youtu.be/S7fIqu1LgJw
* A bridge is defined as an edge which, when removed, makes the graph disconnected \(or increase connected components\)
* O\(E + V\) algorithm

```cpp
int timer = 0;
vector<bool> visited;
vector<int> tin, low;   // -> tin is time we enter that node (in time helps in maintaing ancesstor parent relation)
                        // -> low is lowest ancesstor which can be reached from that node
vector<pii> bridges;
void DFS(int u, vector<int> adj[], int par = -1)
{
    visited[u] = true;
    tin[u] = low[u] = timer++;
    for (auto &v : adj[u])
    {
        if (v == par) continue;
        if (visited[v]) low [u] = min(low[u], tin[v]);      // back edge, so it cannot be a bridge
        else
        {
            DFS(v, adj, u);
            low[u] = min(low[u], low[v]);
            if (low[v] > tin[u]) bridges.push_back({u, v});
        }
    }
}
void findBridge(int n, vector<int> adj[])
{
    timer = 0;
    bridges.clear();
    visited.assign(n, false);
    tin.assign(n, -1);
    low.assign(n, -1);
    for (int i = 0; i < n; ++i)
        if (!visited[i]) DFS(i, adj);
}
```

### Finding Articulation Points

* An articulation point \(or cut vertex\) is defined as a vertex which, when removed along with associated edges, makes the graph disconnected.
* O\(E + V\) algorithm

```cpp
int timer = 0;
vector<bool> visited;
vector<int> tin, low;
vector<int> articulationPoints;
void DFS(int u, vector<int> adj[], int par = -1)
{
    visited[u] = true;
    tin[u] = low[u] = timer++;
    int children = 0;
    for (auto &v : adj[u])
    {
        if (v == par) continue;
        if (visited[v]) low [u] = min(low[u], tin[v]);
        else
        {
            DFS(v, adj, u);
            low[u] = min(low[u], low[v]);
            if (low[v] > tin[u] && par != -1) articulationPoints.push_back(u);
            ++children;
        }
    }
    if (par == -1 && children > 1) articulationPoints.push_back(u);
}
void findArticulationPoint(int n, vector<int> adj[])
{
    timer = 0;
    articulationPoints.clear();
    visited.assign(n, false);
    tin.assign(n, -1);
    low.assign(n, -1);
    for (int i = 0; i < n; ++i)
        if (!visited[i]) DFS(i, adj);
}
```

### Multi-Source BFS

Rotting Oranges

```cpp
class Solution {
public:
    typedef pair<int, int> pii;
    vector<pii> dir = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int orangesRotting(vector<vector<int>>& grid)
    {
        int n = grid.size(), m = grid[0].size();
        queue<pii> q;
        int cnt = 0;
        for (int i = 0; i < n; ++i)
        {
            for (int j = 0; j < m; ++j)
            {
                if (grid[i][j] == 2) q.push({i, j});
                else if (grid[i][j] == 1) cnt++;
            }
        }
        int iter = 0;
        while (!q.empty())
        {
            int sz = q.size();
            iter++;
            while (sz--)
            {
                auto u = q.front(); q.pop();
                for (const pii delta : dir)
                {
                    pii v = {u.first+delta.first, u.second+delta.second};
                    if (v.first >= 0 && v.second >= 0 && v.first < n && v.second < m
                        && grid[v.first][v.second] == 1)
                    {
                        cnt--;
                        grid[v.first][v.second] = 2;
                        q.push({v.first, v.second});
                    }
                }
            }
            if (cnt == 0)
            for (auto &x : grid)
            {
                for (auto &y : x) cout << setw(3) << y;
                cout << '\n';
            }
            cout << '\n';
        }
        return iter;
    }
};
```

### Bidirectional BFS
- [Word Ladder](https://leetcode.com/problems/word-ladder/)
```c++
// Slow normal BFS
int ladderLength(string beginWord, string endWord, vector<string>& wordList)
{
    int m = beginWord.size();   // since all words are of same length
    unordered_map<string, vector<string>> preprocess;
    for (auto x : wordList)
        for (int i = 0; i < m; ++i)
            preprocess[x.substr(0, i) + '*' + x.substr(i+1)].push_back(x);

    queue<pair<string, int>> q;
    q.push({beginWord, 1});
    unordered_set<string> vis;  // vis to make sure we dont repeat same word
    vis.insert(beginWord);
    while (!q.empty())
    {
        auto cur = q.front(); q.pop();
        if (cur.first == endWord) return cur.second;
        for (int i = 0; i < m; ++i)
        {
            string tmp = cur.first.substr(0, i) + '*' + cur.first.substr(i+1);
            for (auto &nxt : preprocess[tmp])
            {
                if (vis.find(nxt) == vis.end())
                {
                    vis.insert(nxt);
                    q.push({nxt, cur.second+1});
                }
            }
        }
    }
    return 0;
}
// Further optimization Bi directional BFS
int ladderLength(string beginWord, string endWord, vector<string>& wordList)
{
    unordered_set<string_view> rec(wordList.begin(), wordList.end());
    if (rec.find(endWord) == rec.end()) return 0;

    unordered_set<string_view> s1{beginWord}, s2{endWord};
    rec.erase(beginWord); rec.erase(endWord);

    int res = 1;
    while (!s1.empty() && !s2.empty())
    {
        /* make sure we always expand the smaller side, thus 
            reduce the overall size of the 2 predecessor tree */
        if (s1.size() > s2.size()) swap(s1, s2);

        unordered_set<string_view> tmp;
        for (const auto word : s1)
        {
            string wordCopy = word.data();
            for (auto &ch : wordCopy)
            {
                char orig = ch;
                for (ch = 'a'; ch <= 'z'; ++ch)
                {
                    if (s2.find(wordCopy) != s2.end()) return res+1;
                    if (rec.find(wordCopy) == rec.end()) continue;

                    const auto it = rec.find(wordCopy);
                    tmp.insert(*it); rec.erase(*it);
                }
                ch = orig;
            }
        }
        s1 = tmp;
        ++res;
    }
    return 0;
}
```

### [Open the lock](https://leetcode.com/problems/open-the-lock/)
```c++
int openLock(vector<string>& deadends, string target)
{
    unordered_set<string> rec(deadends.begin(), deadends.end()), vis;

    queue<string> q;
    q.push("0000");
    int cnt = 0;
    while (!q.empty())
    {
        int sz = q.size();
        while (sz--)
        {
            string u = q.front(); q.pop();
            if (rec.find(u) != rec.end() || vis.find(u) != vis.end()) continue;
            if (u == target) return cnt;
            vis.insert(u);

            for (auto &ch : u)
            {
                char orig = ch;
                for (const int dir : {-1, 1})
                {
                    ch = (((orig-'0')+dir+10) % 10) + '0';
                    q.push(u);
                }
                ch = orig;
            }
        }
        cnt++;
    }
    return -1;        
}

// Bi directional BFS Optimization
int openLock(vector<string>& deadends, string target)
{
    unordered_set<string> rec(deadends.begin(), deadends.end()), vis;
    queue<string> q1, q2;
    unordered_set<string> r1, r2;
    q1.push("0000"); q2.push(target); r1.insert("0000"); r2.insert(target);

    int cnt = 0;
    while (!q1.empty() && !q2.empty())
    {
        if (q1.size() > q2.size()) swap(q1, q2), swap(r1, r2);
        int sz = q1.size();
        while (sz--)
        {
            string u = q1.front(); q1.pop();
            if (rec.find(u) != rec.end() || vis.find(u) != vis.end()) continue;
            if (r2.find(u) != r2.end()) return cnt;
            vis.insert(u);

            for (auto &ch : u)
            {
                char orig = ch;
                for (const int dir : {-1, 1})
                {
                    ch = (((orig-'0')+dir+10) % 10) + '0';
                    q1.push(u);
                    r1.insert(u);
                }
                ch = orig;
            }
        }
        cnt++;
    }
    return -1;
}
```

[https://codeforces.com/problemset/problem/1294/F](https://codeforces.com/problemset/problem/1294/F)  
We want to find 3 points as far away as possible, two of them can be diameter of the tree third one we can by using Multisource BFS with sources \(dA and dB\) then finding farthest point

```cpp
if (diameter.size() == n) // graph is like linkedlist so pick anything as third
    cout << n-1 << '\n' << diameter[0]+1 << ' ' << diameter[1]+1 << ' ' <<
        diameter.back()+1 << '\n';
else
{
    queue<int> q;
    vector<int> d(n, -1);
    for (auto &x : diameter) d[x] = 0, q.push(x);
    while (!q.empty())
    {
        auto u = q.front();
        q.pop();
        for (auto &v : adj[u])
        {
            if (d[v] != -1) continue;
            d[v] = d[u] + 1;
            q.push(v);
        }
    }
    pii mx = {d[0], 0};
    for (int i = 1; i < n; ++i) mx = max(mx, {d[i], i});
    cout << diameter.size()-1+mx.first << '\n' << diameter[0]+1 << ' ' <<
        mx.second+1 << ' ' << diameter.back()+1 << '\n';
}
```

### [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

```cpp
/* Observation: If a 'O' is not captured by 'X' then it has to be connected to the boundary.
So apply DFS and do the thing. */
int n, m;
vector<pair<int, int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
void dfs(vector<vector<char>>& board, int i, int j)
{
    if (i < 0 || j < 0 || i >= n || j >= m || board[i][j] != 'O') return;
    board[i][j] = '+';
    for (const auto dir : directions)
        dfs(board, i+dir.first, j+dir.second);
}
void solve(vector<vector<char>>& board)
{
    if (board.empty() || board[0].empty()) return;
    n = board.size(), m = board[0].size();
    for (int i = 0; i < n; ++i) { dfs(board, i, 0); dfs(board, i, m-1); }
    for (int j = 1; j < m-1; ++j) { dfs(board, 0, j); dfs(board, n-1, j); }
    for (int i = 0; i < n; ++i)
    {
        for (int j = 0; j < m; ++j)
        {
            if (board[i][j] == '+') board[i][j] = 'O';
            else if (board[i][j] == 'O') board[i][j] = 'X';
        }
    }
}
```

### Bidirectional BFS

[https://leetcode.com/problems/open-the-lock/](https://leetcode.com/problems/open-the-lock/)

```cpp
// Normal BFS
class Solution {
public:
    int openLock(vector<string>& deadends, string target)
    {
        unordered_set<string> stops;
        for (const string x : deadends) stops.insert(x);

        queue<string> q;
        unordered_set<string> vis;
        q.push("0000");
        int cnt = 0;
        while (!q.empty())
        {
            int sz = q.size();
            while (sz--)
            {
                string u = q.front(); q.pop();
                if (stops.find(u) != stops.end() || vis.find(u) != vis.end()) continue;
                if (u == target) return cnt;
                vis.insert(u);

                for (int i = 0; i < 4; ++i)
                {
                    string v = u;
                    v[i] = '0' + ((u[i]-'0')+1)%10;
                    if (stops.find(v) == stops.end()) q.push(v);
                    v[i] = '0' + ((u[i]-'0')-1+10)%10;
                    if (stops.find(v) == stops.end()) q.push(v);
                }
            }
            cnt++;
        }
        return -1;
    }
};

// Bidirectional BFS optimization
```

## Shortest Paths

### Bellman-Ford Algorithm

* Finds shortest path from starting node to all other nodes in **O\(VE\)**
* Doesn't work with negative cycle \(works on negative edges unlike Dijikstra, negative cycle is the cycle with total sum negative\)
* Relaxes graph n-1 times, in each relaxation picks every edge and minimises u-&gt;v path distance.
* If the graph contains a negative cycle, we can shorten infinitely many times any path that contains the cycle by repeating the cycle again and again. Thus, the concept of a shortest path is not meaningful in that situation.

```cpp
struct edge { int u, v, w; };
#define INF (1LL<<61)
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, m; cin >> n >> m;
    vec<1, edge> edges(m);
    for (auto &x : edges) cin >> x.u >> x.v >> x.w;

    vec<1, int> dist(n+1, INF), par(n+1, -1);
    dist[1] = 0;
    int x;
    for (int i = 0; i < n; ++i)
    {
        x = -1;
        for (auto &e : edges)
        {
            if (dist[e.u] + e.w < dist[e.v])
            {
                dist[e.v] = dist[e.u] + e.w;
                par[e.v] = e.u;
                x = e.v;
            }
        }
    }

    if (x == -1) cout << "NO\n";
    else
    {
        cout << "YES\n";
        for (int i = 0; i < n; ++i) x = par[x];
        vec<1, int> cycle;
        for (int v = x; ; v = par[v])
        {
            cycle.push_back(v);
            if (v == x && cycle.size() > 1) break;
        }
        reverse(all(cycle));
        for (auto &x : cycle) cout << x << " ";
        cout << '\n';
    }
    return 0;
}

/* Usually we get ans in few phases so terminating early will be better
so introducing a 'changes' check to detect if there's any change can speed up */
vector<int> findPath(int to, vector<int> &prev)
{
    vector<int> path;
    for (int cur = to; cur != -1; cur = prev[cur])
        path.push_back(cur);
    reverse(path.begin(), path.end());
    return path;
}
```

**Proof:**

For all unreachable vertices algorithm works fine.

Let us now prove the following assertion: After the execution of ith phase, the Bellman-Ford algorithm correctly finds all shortest paths whose number of edges does not exceed i. Also any shortest path cannot have more than n-1 vertices so relaxing atmost n-1 times should do it.

![](.gitbook/assets/image%20%28112%29%20%281%29.png)

### SPFA Algorithm

* Fastest algorithm source - all nodes shortest path, average case it's linear however worst case same as Bellman-Ford.
* The algorithm maintains a queue of nodes that is used for reducing the distances. First, the algorithm adds the starting node x to the queue. Then, the algorithm always processes the first node in the queue, and when an edge a→b reduces a distance, node b is added to the queue.

```cpp
bool spfa(int s, int n, vector<pii> adj[], vector<int> &dist)
{
    dist.assign(n, INF);
    vector<int> cnt(n, 0);
    vector<bool> inqueue(n, false);
    queue<int> q;
    dist[s] = 0;
    q.push(s);
    inqueue[s] = true;
    while (!q.empty())
    {
        int u = q.front();
        q.pop();
        inqueue[u] = false;
        for (auto &edge : adj[u])
        {
            int v = edge.first, w = edge.second;
            if (dist[u] + w < dist[v])
            {
                dist[v] = dist[u] + w;
                if (!inqueue[v])
                {
                    q.push(v);
                    inqueue[v] = true;
                    cnt[v]++;
                    if (cnt[v] > n) return false;	// negative cycle
                }
            }
        }
    }
    return true;
}
```

### Dijkstra Algorithm

![](.gitbook/assets/image%20%2890%29%20%281%29.png)

* Priority queue version is O\(E logV\)
* A remarkable property in Dijkstra’s algorithm is that whenever a node is selected, its distance is final.

```cpp
void dijkstra(int s, int n, vector<pii> adj[], vector<int> &dist, vector<int> &prev)
{
    dist.assign(n, INF);
    prev.assign(n, -1);
    dist[s] = 0;
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    pq.push({0, s});
    while (!pq.empty())
    {
        int u = pq.top().second, d = pq.top().first;
        pq.pop();
        if (d != dist[u]) continue;
        for (auto &edge : adj[u])
        {
            int v = edge.first, w = edge.second;
            if (dist[u] + w < dist[v])
            {
                dist[v] = dist[u] + w;
                prev[v] = u;
                pq.push({dist[v], v});
            }
        }
    }
}
```

### 0-1 BFS (Zero One BFS)

* https://youtu.be/cMP1IaWuFuM
* During the execution of BFS, the queue holding the vertices only containing elements from at max two successive levels of the BFS tree.
* Used in unweighted graph \(or 0-1 weight graph\), performs in O\(E\)

```cpp
void zeroOneBFS(int s, int n, vector<pii> adj[], vector<int> &dist, vector<int> &prev)
{
    dist.assign(n, INF);
    prev.assign(n, -1);
    dist[s] = 0;
    deque<int> dq;
    dq.push_front(s);
    while (!dq.empty())
    {
        int u = dq.front();
        dq.pop_front();
        for (auto &edge : adj[u])
        {
            int v = edge.first, w = edge.second;
            if (dist[u] + w < dist[v])
            {
                dist[v] = dist[u] + w;
                prev[v] = u;
                if (w == 1) dq.push_back(v);
                else dq.push_front(v);
            }
        }
    }
}
```

### Floyd Warshall Algorithm

```cpp
for (int k = 0; k < n; ++k)
{
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            if (dist[i][k] < INF && dist[k][j] < INF)
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
}
```

* [Greg and Graph](https://codeforces.com/contest/296/problem/D) - [https://codeforces.com/contest/296/submission/74248854](https://codeforces.com/contest/296/submission/74248854)

## Disjoint Set Union

```cpp
int parent[1000];
void makeSet(int x) { parent[x] = x; }
int findSet(int x) { return (x == parent[x]) ? x : findSet(parent[x]); }
void unionSet(int x, int y)
{
    x = findSet(x), y = findSet(y);
    if (x != y) parent[y] = x;
}
// In worst case it's linear in nature (in case of long chain)

/* If we call find_set(v) for some vertex v, we actually find
the representative p for all vertices that we visit on the path
between v and the actual representative p. The trick is to make
the paths for all those nodes shorter, by setting the parent of
each visited vertex directly to p. */

// Path compression modification in findSet()
int findSet(int x) { return (x == parent[x]) ? x : parent[x] = findSet(parent[x]); }
// This makes avg logN

/* Next optimization is in union operation, In the naive
implementation the second tree always got attached to the first
one. In practice that can lead to trees containing chains of length
O(n). So we will have a size field storing size of subtree and we
will append to make our tree shorter */

// Final optimized implementation using path compression
int parent[1000], sz[1000];
void makeSet(int x) { parent[x] = x, sz[x] = 1; }
int findSet(int x) { return (x == parent[x]) ? x : parent[x] = findSet(parent[x]); }
void unionSet(int x, int y)
{
    x = findSet(x), y = findSet(y);
    if (x != y)
    {
        // size compression
        if (sz[x] < sz[y]) swap(x, y);
        parent[y] = x;
        sz[x] += sz[y];
        /*
        // Rank Compression
        if (rank[x] < rank[y]) swap(x, y);
        parent[y] = x;
        if (rank[x] == rank[y]) rank[x]++;
        */
    }
}
```

If we combine both optimizations path compression & union by size we will reach nearly constant time queries. It's O\(α\(n\)\), where α\(n\) is the inverse Ackermann function, which grows very slowly. Worst case it's logN.

* **Connected components in a graph** - Initially we have an empty graph. We have to add vertices and undirected edges, and answer queries of the form \(a,b\) - "are the vertices a and b in the same connected component of the graph?" - Used by Kriskal MST algo
* **Search for connected components in an image -** there is an image of n×m pixels. Originally all are white, but then a few black pixels are drawn. You want to determine the size of each white connected component in the final image. Iterate all n\*m cells and if it's white iterate to it's neighbour if they are also white then union them. Thus resulting tree in DSU are desired connected components.
* **Painting subarrays offline -** Given n length array and m queries \(specifying l, r and c\) we are painting from l to r to color c. Now we want to tell final color. Solution is start in reverse of query because what's colored in ith will never get recolored before i. We are basically marking components from l to r. x = findset\(x\) ensures we skip components which are colored before. parent\[x\] = x+1 moves the array.

```cpp
int n, m; cin >> n >> m;
for (int i = 0; i <= n; ++i) makeSet(i);
vector<pair<pii, int>> query(m);
for (auto &x : query) cin >> x.F.F >> x.F.S >> x.S;
int ans[n] {};
for (int i = m-1; i >= 0; --i)
{
    int l = query[i].F.F-1, r = query[i].F.S-1, c = query[i].S;
    for (int x = findSet(l); x <= r; x = findSet(x))
        ans[x] = c, parent[x] = x+1;
}
```

### [Most Stones Removed with Same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/)

create a graph connecting edge i & j such that stone\[i\] and stone\[j\] are either in same row or same column.

Idea is that we can reduce every components \(formed in such a graph\) to 1 node. So N - 1\*components should be the answer

```cpp
vector<int> parent;
void makeSet(int x) { parent[x] = x; }
int findSet(int x) { return x == parent[x] ? x : parent[x] = findSet(parent[x]); }
void unionSet(int x, int y)
{
    x = findSet(x), y = findSet(y);
    if (x != y) parent[y] = x;
}

int removeStones(vector<vector<int>>& stones)
{
    unordered_map<int, vector<int>> row, col;
    int n = stones.size();
    for (int i = 0; i < n; ++i)
    {
        row[stones[i][0]].push_back(i);
        col[stones[i][1]].push_back(i);
    }
    parent.resize(n);
    for (int i = 0; i < n; ++i) makeSet(i);
    for (int i = 0; i < n; ++i)
    {
        for (int j : row[stones[i][0]]) unionSet(i, j);
        for (int j : col[stones[i][1]]) unionSet(i, j);
    }
    unordered_set<int> res;
    for (const int x : parent) res.insert(findSet(x));
    return n - res.size();
}
```

### [Redundant Connection]
- https://leetcode.com/problems/redundant-connection/
- https://leetcode.com/problems/redundant-connection-ii/
```c++
// Variant - I (Undirected graph)
vector<int> findRedundantConnection(vector<vector<int>>& edges)
{
    parent = vector<int>(edges.size()+1);
    sz = vector<int>(edges.size()+1, 1);
    for (int i = 1; i <= edges.size(); ++i) parent[i] = i;

    vector<int> res;
    for (const auto e : edges)
    {
        if (findSet(e[0]) != findSet(e[1])) unionSet(e[0], e[1]);
        else res = e;
    }
    return res;
}

// Variant - II (Directed graph)
vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges)
{
    parent = vector<int>(edges.size()+1);
    sz = vector<int>(edges.size()+1, 1);
    for (int i = 1; i <= edges.size(); ++i) parent[i] = i;
    
    vector<int> inDeg(edges.size()+1, 0);
    int node = -1;
    for (const auto e : edges)
    {
        inDeg[e[1]]++;
        if (inDeg[e[1]] == 2) { node = e[1]; break; }
    }

    if (node == -1)     // Directed cycle must be their
    {
        for (const auto e : edges)
        {
            if (findSet(e[0]) != findSet(e[1])) unionSet(e[0], e[1]);
            else return {e[0], e[1]};
        }
    }
    else                // There's a node with inDeg 2
    {
        int a = -1, b = -1;
        for (const auto e : edges)
        {
            if (e[1] != node) unionSet(e[0], e[1]);
            else if (a == -1) a = e[0];
            else b = e[0];
        }
        if (findSet(a) == findSet(node)) return {a, node};
        else return {b, node};
    }
    return {};
}
```

### [Evaluate Division](https://leetcode.com/problems/evaluate-division/)

Given a list of form a/b = some\_double, find answer for query list of form p/q. Return -1 if not possible for that query.

```cpp
/* idea is if given some a/b value we are essentially grouping a and b i.e. we can find b/a aswell with
this piece of information. Say given a/b b/c we can find a/c also. So we should group them together
to maintain a record.
When there's a new edge we can associate vals to its individual nodes initially v : 1. We have to
maintain whole other subtree in case it mismatches. Example

a/b = 3       e/f = 2       (b is 1.5 assuming, a is 4.5, f = 6, e = 3)     b/e = 1.5/3 = 0.5

   (3) a ---> b (1)        (2) e ---> f (1)         After first 2 insertion
   
   Inserting b ---> e requires changing it's corresponding subtree aswell
   For finding a value we keep multiplying entire subtree, so value of a is 3*1, value of e is 2*1
   so we move to the last node i.e. b & f and connect b ---> f instead and change their value.
   va will become v * vb/va i.e. 0.5 * (2/1)
   
   (3) a ---> b (1)        (2) e ---> f (1)
                \--------------------/
    
    There could be multiple possible values so a, b might not correspond to the one we
    assumed initially, but it's correct
*/
unordered_map<string, pair<string, double>> rec;
void makeSet(string x) { rec[x] = {x, 1.0}; }
string findSet(string x, double &val)
{
    while (rec[x].first != x)
    {
        val *= rec[x].second;
        x = rec[x].first;
    }
    return x;
}
void unionSet(string x, string y, double val)
{
    double vx = 1.0, vy = 1.0;
    x = findSet(x, vx), y = findSet(y, vy);
    if (x != y) rec[x] = {y, val * vy/vx};
}

vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries)
{
    rec.clear();
    for (int i = 0; i < equations.size(); ++i)
    {
        string a = equations[i][0], b = equations[i][1];
        if (rec.find(a) == rec.end()) makeSet(a);
        if (rec.find(b) == rec.end()) makeSet(b);
        unionSet(a, b, values[i]);
    }
    vector<double> res;
    for (auto &x : rec)
        cout << x.first << " " << x.second.first << " " << x.second.second << '\n';
    for (const auto q : queries)
    {
        if (rec.find(q[0]) == rec.end() || rec.find(q[1]) == rec.end())
            res.push_back(-1);
        else
        {
            double va = 1.0, vb = 1.0;
            if (findSet(q[0], va) == findSet(q[1], vb)) res.push_back(va/vb);
            else res.push_back(-1);
        }
    }
    return res;
}
```

* [Path Queries](https://codeforces.com/contest/1213/problem/G): Consider sorted edges \(according to weight\) to find ans for an arbitary weight w we have to consider DSU unioned tree keep choosing 2 from the chosen, to choose 2 we can simply modify our DFS union set to increment cnt += sz\[x\] + sz\[y\] [https://codeforces.com/contest/1213/submission/78358873](https://codeforces.com/contest/1213/submission/78358873)

## Spanning Trees

A spanning tree of a graph consists of all nodes of the graph and some of the edges of the graph so that there is a path between any two nodes. Like trees. A minimum spanning tree is a spanning tree whose weight is as small as possible.

### Kruskal's Algorithm

* Finds MST in O\(E logV\) mostly used over Prim's because implementation is small

```cpp
for (int i = 0; i < n; ++i) makeSet(i);
sort(edges.begin(), edges.end(), [](Edge a, Edge b){ return a.w < b.w; });
for (auto &edge : edges)
{
    if (findSet(edge.u) != findSet(edge.v))
    {
        cost += edge.w;
        res.push_back(edge);
        unionSet(edge.u, edge.v);
    }
}
```

### Prim's Algorithm

* Finds MST in O\(N^2\) however using set \(or heap\) we can find minimum edge in logn time making it O\(E logV\)

```cpp
set<Edge> q;    // Edge has w & to
st.insert({0, 0});
vector<bool> selected(n, false);
vector<Edge> minE(n);
minE[0].w = 0;

for (int i = 0; i < n; ++i)
{
    if (st.empty()) { cout << "NO MST!\n"; return 0; }
    int v = st.begin()->to;
    selected[v] = true;
    cost += st.begin()->w;
    st.erase(st.begin());
    if (minE[v].to != -1) cout << v << " " << minE[v].to << '\n';
    for (auto &edge : adj[v])
    {
        if (!selected[edge.to] && edge.w < minE[edge.to].w)
        {
            st.erase({minE[edge.to].w, edge.to});
            minE[edge.to] = {edge.w, v};
            st.insert({edge.w, v});
        }
    }
}
```

## Directed Graphs

In Directed graphs Topological sort is very important, in directed acyclic concept of DP can be applied

### Topological Sort

```cpp
// General Approach
queue<int> ans;
void DFS(int u)
{
    visited[u] = true;
    for (int v : adj[u])
        if (!visited[v]) DFS(v);
    ans.push(u);
}
for (int i = 0; i < n; ++i)
    if (!visited[i]) DFS(i);
while (!ans.empty()) { cout << ans.front() << '\n'; q.pop(); }

// Kanh's Algorithm
queue<int> q;
for (int i = 0; i < n; ++i)
    if (inDeg[i] == 0) q.push(i);
vector<int> topOrder;
while (!q.empty())
{
    auto u = q.front(); q.pop();
    topOrder.push_back(u);
    for (auto &v : adj[u])
    {
        inDeg[v]--;
        if (inDeg[v] == 0) q.push(v);
    }
}

// Can check if graph is cyclic by topOrder.size() != n
```

#### [Alien Dictionary](https://www.lintcode.com/problem/alien-dictionary/description)

```cpp
string alienOrder(vector<string> &words)
{
    unordered_map<char, int> inDeg;
    for (auto &x : words) for (auto &y : x) inDeg[y] = 0;
    unordered_map<char, vector<char>> adj;
    for (int i = 0; i < words.size()-1; ++i)
    {
        string word1 = words[i], word2 = words[i+1];
        for (int j = 0; j < min(word1.size(), word2.size()); ++j)
            if (word1[j] != word2[j]) { adj[word1[j]].push_back(word2[j]), inDeg[word2[j]]++; break; }
    }
    
    priority_queue<char, vector<char>, greater<char>> q;
    for (auto &x : inDeg)
        if (x.second == 0) q.push(x.first);
    string res;
    while (!q.empty())
    {
        auto u = q.top(); q.pop();
        res += u;
        for (auto &v : adj[u])
        {
            inDeg[v]--;
            if (inDeg[v] == 0) q.push(v);
        }
    }
    return (res.size() == inDeg.size()) ? res : "";
}
```

### Counting the number of paths

![Let&apos;s calculate number of paths from node 1 to node 6](.gitbook/assets/image%20%2876%29.png)

![](.gitbook/assets/image%20%2844%29.png)

```cpp
void DFS(int u)
{
    DP[u]++;
    for (auto &v : adj[u]) DFS(v);
}
DFS(s);
cout << DP[d] << '\n;
```

### Extending Dijkstra's Algorithm

By product of Dijkstra algorithm is a DAG signifying considered shortest path edges. DP can be applied to find number of shortest paths from source to destination like above.

![](.gitbook/assets/image%20%28136%29.png)

### Successor Paths / Functional Graph

* Such graphs have outdegree as 1 \(also called functional graph since it defines a function\)

![succ\(4,6\) = 2 i.e. move 6 units from point 4](.gitbook/assets/image%20%28108%29%20%281%29.png)

In finding succ\(x, k\) it takes O\(k\) time however with preprocessing query becomes O\(logK\)

![](.gitbook/assets/image%20%2854%29.png)

```cpp
// spraseTable[0] will represent succ array
// For query, present k as sum of powers of 2 so 11 = 8 + 2 + 1
// succ(x,11) = succ(succ(succ(x,8),2),1)
const int MAXN = 2*1e5 + 5;
vec<2, int> succ(32, MAXN);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    for (int j = 1; j <= n; ++j) cin >> succ[0][j];
    for (int i = 1; i < 32; ++i)
        for (int j = 1; j <= n; ++j)
            succ[i][j] = succ[i-1][succ[i-1][j]];
 
    while (q--)
    {
        int x, k; cin >> x >> k;
        int i = 0;
        while (k)
        {
            if (k&1) x = succ[i][x];
            k >>= 1;
            ++i;
        }
        cout << x << '\n';
    }
    return 0;
}

// Find kth parent of a node in tree: https://cses.fi/problemset/task/1687
const int MAXN = 2*1e5 + 5;
vec<2, int> par(32, MAXN);
signed main()
{
    ios_base::sync_with_stdio(false); cin.tie(NULL);
    int n, q; cin >> n >> q;
    par[0][1] = 0;
    for (int j = 2; j <= n; ++j) cin >> par[0][j];
    for (int i = 1; i < 32; ++i)
        for (int j = 1; j <= n; ++j)
            par[i][j] = par[i-1][par[i-1][j]];

    while (q--)
    {
        int x, k; cin >> x >> k;
        int i = 0;
        while (k)
        {
            if (k&1) x = par[i][x];
            k >>= 1;
            ++i;
        }
        if (x == 0) x = -1;
        cout << x << '\n';
    }
    return 0;
}
```

## Trees

### Finding kth ancestors

Above logic in successor paths can be applied here

![](.gitbook/assets/image%20%2824%29.png)

![](.gitbook/assets/image%20%28166%29.png)

### Diameter of a tree

* Maximum length of a path between two nodes. \(There could be multiple diameters possible\)
* **Algorithm 1:** A general way to approach many tree problems is to first root the tree arbitrarily, after that solve the problem separately for each subtree. For each node we find: toLeaf\(x\): max length of path from x to any leaf. maxLength\(x\): max length of path whose highest point is x.

![](.gitbook/assets/image%20%2888%29.png)

```cpp
int ans = 0;
int dfs(int u = 1, int par = -1)
{
    int dep1 = 0, dep2 = 0;
    for (const int v : adj[u])
    {
        if (v == par) continue;
        int curDep = dfs(v, u);
        if (curDep > dep1) dep2 = dep1, dep1 = curDep;
        else if (curDep > dep2) dep2 = curDep;
    }
    ans = max(ans, dep1+dep2);
    return dep1+1;
}
```

* **Algorithm 2:** Another way is using 2 DFS. Go to deepest point from any root using DFS then from that deepest point again apply DFS this will be our second point of diameter.

```cpp
const int N = 2*1e5;
int n;
vector<int> adj[N], parent(N);
void DFS(int u, pii &res, int par = -1, int dist = 0)
{
    parent[u] = par;
    res = max(res, {dist, u});
    for (auto &v : adj[u])
    {
        if (v == par) continue;
        DFS(v, res, u, dist+1);
    }
}
vector<int> findDiameter(pii &pointA, pii &pointB)
{
    DFS(0, pointA); DFS(pointA.second, pointB);
    vector<int> diameter;
    for (int cur = pointB.second; cur != pointA.second; cur = parent[cur])
        diameter.push_back(cur);
    diameter.push_back(pointA.second);
    return diameter;
}
```

### All Longest Paths

* Find for every node, the maximum length of path that begins at that node in O\(n\). This can be seen as genralization of above diameter problem.
* Not so optimal but easy to understand strategy is to pick every leaf nodes and find distances considering it as root.

![Here we are finding length \(edges\), we can generally find nodes and subtract it by 1](.gitbook/assets/image%20%2894%29.png)

```cpp
int n;
vector<vector<int>> adj;
vector<int> dist;
void dfs(int u, int par = -1)
{
    for (const int v : adj[u])
    {
        if (v == par) continue;
        dist[v] = dist[u]+1;
        dfs(v, u);
    }
}
vector<int> allLongestPath()
{
    vector<int> res(n+1, 0);
    dfs(1);

    int optimal = 0;
    for (int i = 1; i <= n; ++i)
        if (dist[i] > dist[optimal]) optimal = i;
    fill(dist.begin(), dist.end(), 0);
    dfs(optimal);
    for (int i = 1; i <= n; ++i) res[i] = dist[i];

    for (int i = 1; i <= n; ++i)
        if (dist[i] > dist[optimal]) optimal = i;
    fill(dist.begin(), dist.end(), 0);
    dfs(optimal);
    for (int i = 1; i <= n; ++i) res[i] = max(res[i], dist[i]);

    return res;
}
```

### Re-rooting technique

[https://cses.fi/problemset/task/1133](https://cses.fi/problemset/task/1133)
For each node in tree find sum of distance to all other nodes
```c++
int n;
vector<vector<int>> adj;
vector<int> sz;
void dfs(vector<int> &res, int u = 1, int par = -1, int dis = 0)
{
    sz[u] = 1, res[1] += dis;
    for (const int v : adj[u])
    {
        if (v == par) continue;
        dfs(res, v, u, dis+1);
        sz[u] += sz[v];
    }
}
void dfs2(vector<int> &res, int u = 1, int par = -1)
{
    for (const int v : adj[u])
    {
        if (v == par) continue;
        res[v] = res[u] + (n - sz[v]) - sz[v];
        dfs2(res, v, u);
    }
}
vector<int> allPathSumForEveryNode()
{
    vector<int> res(n+1, 0);
    dfs(res);          // fills sz for each node and find res for 1st (root) node
    dfs2(res);         // finds actual res using re rooting technique
    return res;
}
```

[https://atcoder.jp/contests/abc160/tasks/abc160\_f](https://atcoder.jp/contests/abc160/tasks/abc160_f)

```cpp
/* dp[u] = (cnt[u] - 1)! * (dp[v0]/cnt[v0]!) * (dp[v1]/cnt[v1]!)
After that applying rerooting technique */
vector<int> adj[MAXN+1];
int cnt[MAXN+1], dp[MAXN+1];
vector<int> adj[MAXN+1];
int cnt[MAXN+1], dp[MAXN+1];
void precompute(int u, int par = -1)
{
    cnt[u] = 1, dp[u] = 1;
    for (auto &v : adj[u])
    {
        if (v == par) continue;
        precompute(v, u);
        cnt[u] += cnt[v];
        dp[u] = multiply(dp[u], divide(dp[v], fact[cnt[v]]));
    }
    dp[u] = multiply(dp[u], fact[cnt[u]-1]);
}
void reroot(int u, int par = -1)
{
    cout << dp[u] << '\n';
    for (auto &v : adj[u])
    {
        if (v == par) continue;
        int prev_dp_u = dp[u], prev_dp_v = dp[v], prev_cnt_u = cnt[u], prev_cnt_v = cnt[v];
        dp[u] = divide(dp[u], dp[v]);
        dp[u] = multiply(dp[u], fact[cnt[v]]);
        dp[u] = divide(dp[u], fact[cnt[u]-1]);
        dp[v] = divide(dp[v], fact[cnt[v]-1]);
        cnt[u] -= cnt[v];
        cnt[v] += cnt[u];
        dp[u] = multiply(dp[u], fact[cnt[u]-1]);
        dp[v] = multiply(dp[v], dp[u]);
        dp[v] = multiply(dp[v], fact[cnt[v]-1]);
        dp[v] = divide(dp[v], fact[cnt[u]]);

        reroot(v , u);

        dp[u] = prev_dp_u, dp[v] = prev_dp_v, cnt[u] = prev_cnt_u, cnt[v] = prev_cnt_v;
    }
}
```

[https://codeforces.com/contest/1187/problem/E](https://codeforces.com/contest/1187/problem/E)

Here dp\[u\] = cnt\[u\] + dp\[v0\] + dp\[v1\] + dp\[v2\] ...

```cpp
void reroot(int u, int par = -1)
{
    ans = max(ans, dp[cur]);
    for (auto &v : adj[u])
    {
        if (v == par) continue;
        int prev_dp_u = dp[u], prev_dp_v = dp[v], prev_cnt_u = cnt[u], prev_cnt_v = cnt[v];
        dp[u] -= dp[v];
        dp[u] -= cnt[v];
        cnt[u] -= cnt[v];
        cnt[v] += cnt[u];
        dp[v] += cnt[u];
        dp[v] += dp[u];

        reroot(v , u);

        dp[u] = prev_dp_u, dp[v] = prev_dp_v, cnt[u] = prev_cnt_u, cnt[v] = prev_cnt_v;
    }
}
```

[https://codeforces.com/contest/1324/problem/F](https://codeforces.com/contest/1324/problem/F)

[https://codeforces.com/contest/960/problem/E](https://codeforces.com/contest/960/problem/E)

### Subtree traversal technique

Using the DFS traversed array we are performing certain queries

![](.gitbook/assets/image%20%2855%29%20%281%29.png)

* Each subtree corresponds to subarray, such that first element in root node using this we can - update the value of a node, calculate the sum of values in subtree of a node. Idea is to construct a tree traversal array that contains 3 values for each node -size of subtree and value of node. Now to find say sum of values of node 4 check corresponding size and then sum all those \(as shown in image\). This summation or updation can be done using BIT making query logN

![](.gitbook/assets/image%20%2851%29.png)

* Say now problem is change the value of node, calculate sum of values on a path from root to node. Idea is now to contruct a path sum row from root to that node. When the value of a node increases by x, the sums of all nodes in its subtree increase by x. Now query is O\(1\) but updation is O\(logN\)

![Sum of values from root to 7 is 4+5+5 = 14](.gitbook/assets/image%20%28127%29.png)

![Updating in node 4 by 1 means +1 in entire subtree](.gitbook/assets/image%20%28161%29.png)

* Lowest common ancestor of two nodes of a rooted tree is the lowest node whose subtree contains both the nodes.

![](.gitbook/assets/image%20%28128%29%20%281%29.png)

![LCA\(5, 8\) = element with min depth between that range i.e. 2](.gitbook/assets/image%20%2895%29.png)

* Distances of nodes say in image below between 5 & 8. dist\(a, b\) = depth\(a\) + depth\(b\) - 2\*depth\(c\) where c is LCA of a & b.

![](.gitbook/assets/image%20%28131%29%20%281%29.png)

## Strongly Connectivity

- Brute force is to use Floydd Warshal and check if all pair within component is non INF

### Kosaraju Algorithm

* Find topological ordering like you do using DFS
* Apply DFS over graph transpose in topological ordering those will be SCC (Strongly Connected Component).
* If we do transpose of a SCC graph it will still remain SCC.
* https://youtu.be/Rs6DXyWpWrI

```cpp
const int MAXN = 1e5;
vector<int> adj[MAXN+1], adjRev[MAXN+1], order, component;
vector<bool> used;
int n, e;
void DFS(int u)
{
    used[u] = true;
    for (auto &v : adj[u])
        if (!used[v]) DFS(v);
    order.push_back(u);
}
void DFS2(int u)
{
    used[u] = true;
    component.push_back(u);
    for (auto &v : adjRev[u])
        if (!used[v]) DFS2(v);
}
int main()
{
    cin >> n >> e;
    for (int i = 0; i < e; ++i)
    {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        adjRev[v].push_back(u);
    }
    used.assign(n, false);
    for (int i = 0; i < n; ++i)
        if (!used[i]) DFS(i);
    used.assign(n, false);
    reverse(order.begin(), order.end());
    for (auto &v : order)
    {
        if (used[v]) continue;
        DFS2(v);
        // Process compoenents
        component.clear();
    }
}
```

* [Submergin Islands](http://www.spoj.com/problems/SUBMERGE/)
* [Good Travels](http://www.spoj.com/problems/GOODA/)
* [Lego](http://www.spoj.com/problems/LEGO/)
* [Chef and Round Run](https://www.codechef.com/AUG16/problems/CHEFRRUN)
* [Come and Go](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2938)
* [Calling Circles](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=183)
* [Prove Them All](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=4955)
* [Water Supply](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=4393)
* [Lighting Away](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2870)
* [Trouble in Terrorist Town](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=862&page=show_problem&problem=4805)
* [The Largest Clique](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2299)
* [Trust groups](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2756)
* [Wishmaster](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=4598)
* [True Friends](http://www.spoj.com/problems/TFRIENDS/)
* [Capital City](http://www.spoj.com/problems/CAPCITY/)
* [Scheme](http://codeforces.com/contest/22/problem/E)
* [Ada and Panels](http://www.spoj.com/problems/ADAPANEL/)

### 2SAT Problem

Problem of assigning Boolean values to variable to satisfy a given Boolean formula.

![Example: Find assignment of x1, x2, x3, x4 such that formula is true](.gitbook/assets/image%20%28192%29.png)

Each pair \(a V b\) generates two edges -a -&gt; b and -b -&gt;a

![Corresponding graph of the Boolean formula](.gitbook/assets/image%20%28139%29%20%281%29.png)

* First we construct the graph of implications and find all strongly connected components. Done in O\(n+m\)
* Afterwards we can choose the assignment of x by comparing comp\[x\] and comp\[¬x\]. If comp\[x\]=comp\[¬x\] we return false to indicate that there doesn't exist a valid assignment that satisfies the 2-SAT problem.

```cpp
vector<vector<int>> adj, adjRev;
vector<bool> vis, sol, isTrue;
vector<int> mvStk;
bool noSolution;
int N;
void DFS1(int node)
{
    vis[node] = true;
    for (auto vec : adj[node])
        if (!vis[vec]) DFS1(vec);
    mvStk.push_back(node);
}
void DFS2(int node)
{
    if (isTrue[node]) noSolution = true;
    vis[node] = true;
    isTrue[node^1] = true;
    sol[node/2] = ((node&1) ^ 1);
    for (auto vec : adjRev[node])
        if (!vis[vec]) DFS2(vec);
}
inline int get_node(int x) { return (x > 0) ? (2 * (x-1) + 1) : (2 * (-x-1)); }
// USE BELOW FUNCTIONS ONLY
int initialize(int _n)
{
    N = _n;
    adj.assign(2*N, vector<int>());
    adjRev.assign(2*N, vector<int>());
    vis.assign(2*N, false);
    sol.assign(N, false);
    isTrue.assign(2*N, false);
    mvStk.reserve(2*N);
}
void addEdge(int p, int q)
{
    int a, b;
    a = get_node(-p); b = get_node(q);
    adj[a].push_back(b);
    adjRev[b].push_back(a);

    a = get_node(-q); b = get_node(p);
    adj[a].push_back(b);
    adjRev[b].push_back(a);
}
pair<bool, vector<bool>> solve2SAT()
{
    fill(vis.begin(), vis.end(), 0);
    for (int i = 0; i < 2*N; ++i)
        if (!vis[i]) DFS1(i);
    noSolution = false;
    fill(vis.begin(), vis.end(), 0);
    for (int i = 2*N - 1; i >= 0; --i)
        if (!vis[mvStk[i]] && !vis[mvStk[i] ^ 1]) DFS2(mvStk[i]);
    return {!noSolution, sol};
}
```

* [Rectangles](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3081)
* [The Door Problem](http://codeforces.com/contest/776/problem/D)
* [Radio Stations](https://codeforces.com/problemset/problem/1215/F)

## Paths & Circuits

> Circuit is a path that starts and ends at same node

### Eulerian Path

* Goes through each edge exactly once.
* Efficient algorithm exists.
* An unidirected graph has eulerian path when: 
  * All edge belongs to 1 connected component.
  * Deg of each node is even - \(Euler circuit from every node\) or
  * Deg of exactly 2 nodes is odd and other even - \(Euler path only from 1st odd node to 2nd\).

![Node 1, 3 and 4 have deg 2 and node 2 and 5 have deg 3. \(2-5 Euler path no circuit\)](.gitbook/assets/image%20%28100%29.png)

* A directed graph has eulerian path when: 
  * All edge belongs to 1 connected component.
  * InDeg = OutDeg in each node - \(Euler circuit from every node\) or
  * In 1 node InDeg &gt; OutDeg and in other InDeg &lt; OutDeg rest equal - \(Euler path only from those two nodes\).

![Euler path 2-3-5-4-1-2-5](.gitbook/assets/image%20%28140%29%20%281%29.png)

```cpp
/* Heirholzer's Algorithm to find Euler path in O(V + E) */
const int MAXN = 1e5+5;
struct Edge;
typedef list<Edge>::iterator iter;
vec<1, int> deg(MAXN, 0);
int edgeCnt = 0;
struct Edge
{
    int nextVertex;
    iter reverseEdge;
    Edge(int _nextVertex) : nextVertex(_nextVertex) { }
};
int n;
list<Edge> adj[MAXN+1];
vector<int> path;
void addEdge(int a, int b)
{
    adj[a].push_front(Edge(b));
    iter ita = adj[a].begin();
    adj[b].push_front(Edge(a));
    iter itb = adj[b].begin();
    ita->reverseEdge = itb;
    itb->reverseEdge = ita;
    deg[a]++, deg[b]++;
    edgeCnt++;
}
void helper(int u)
{
    while (adj[u].size() > 0)
    {
        int v = adj[u].front().nextVertex;
        adj[v].erase(adj[u].front().reverseEdge);
        adj[u].pop_front();
        helper(v);
    }
    path.push_back(u);
}
bool findEulerianPathUndirected(int u, int n)
{
    for (auto &x : deg)
        if (x&1) return false;
    helper(u);
    return (path.size() == edgeCnt+1);
}
```

```cpp
vector<int> findEulerianPathDirected(vector<int> adj[], int u)
{
    vector<int> stack, curr_edge(MAXN), res;
    stack.push_back(u);
    while (!stack.empty())
    {
        u = stack.back();
        stack.pop_back();
        while (curr_edge[u] < (int)adj[u].size())
        {
            stack.push_back(u);
            u = adj[u][curr_edge[u]++];            
        }
        res.push_back(u);
    }
    reverse(res.begin(), res.end());
    return res;
}
```

### [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

```cpp
class Solution {
public:
    unordered_map<string, vector<string>> adj;
    unordered_map<string, int> ptr;
    
    void dfs(string u, vector<string> &res)
    {
        while (ptr[u] < adj[u].size())
            dfs(adj[u][ptr[u]++], res);
        res.push_back(u);
    }
    
    vector<string> findItinerary(vector<vector<string>>& tickets)
    {
        adj.clear(); ptr.clear();
        for (const auto x : tickets) adj[x[0]].push_back(x[1]);
        for (auto &x : adj) sort(x.second.begin(), x.second.end());
        vector<string> res;
        dfs("JFK", res);
        reverse(res.begin(), res.end());
        return res;
    }
};
```

{% embed url="https://youtu.be/iPLQgXUiU14" caption="De Bruijn Sequence" %}

A De Bruijn sequence on set \['0', '1'\] of length 2 is 01100 because its substring contains all possible n length from given set i.e. 00 01 10 11

[https://www.geeksforgeeks.org/de-bruijn-sequence-set-1/](https://www.geeksforgeeks.org/de-bruijn-sequence-set-1/)

Every possible string on st of length _n_ appears exactly once as a substring. n = 3 {0, 1} = 0011101000 in O\(K^n\)

* https://leetcode.com/problems/cracking-the-safe/
```cpp
class Solution {
public:
    unordered_set<string> done;
    vector<int> edges;
    void dfs(string node, const int k)
    {
        for (int i = 0; i < k; ++i)
        {
            string cur = node + (char)('0'+i);
            if (done.find(cur) == done.end())
            {
                done.insert(cur);
                dfs(cur.substr(1), k);
                edges.push_back(i);
            }
        }
    }
    
    string crackSafe(int n, int k)  // for n = 3, k = 2 ans is 0011101000
    {
        done.clear(); edges.clear();
        string startingNode = string(n-1, '0');     // 00
        dfs(startingNode, k);
        string res;
        for (auto &x : edges) res += (char)('0' + x);
        res += startingNode;
        return res;
    }
};
```

### Hamiltonian Path

* Goes through each node exactly once.
* NP Hard problem.
* No efficient method is known for testing if a graph contains a Hamiltonian path.
  * Dirac's Theorem: If deg of each node is atleast n/2 graph contains hamiltonian path.
  * Ore's Theorem: If sum of deg of each non-adjacent pair of nodes is at least n the graph contains hamiltonian path.
* Simple way to search for hamiltonian path is to use backtracking O\(n!\) since there are n! different ways to choose the order of n nodes.

### Knight's Tours

A knight’s tour corresponds to a Hamiltonian path in a graph whose nodes represent the squares of the board, and two nodes are connected with an edge if a knight can move between the squares according to the rules of chess.

![One of knight tour in 5x5 board](.gitbook/assets/image%20%28165%29.png)

Solve using backtracking.

> Warnsdorf's rule: simple and effective heuristic for finding a knight’s tour. Idea is to always move the knight so that it ends up in a square where the number of possible moves is as small as possible.

### Power of Adjancey Matrix

![V^4\[2\]\[5\] = 2 i.e. there are 2 paths from 2 to 5 with 4 edges](.gitbook/assets/image%20%2887%29.png)

* [Timofey and a tree](https://codeforces.com/problemset/problem/763/A) - Given a tree of n vertex \(n-1 edges\) also given an array representing color of node i. We want to pick a node and make it root such that all sub tree formed have only one type of color. **Solution:** If we pick a node x such that edge from it goes to y. If x and y are of different colors then it is definite that they can't lie in same subtree means the breaking should occur here so we either take x as root or y as root. Later we also have to check if after picking x condition is valid or after picking y if both fails then print NO. There can be a scenerio in which all nodes are of same color then x & y nodes will not be found we should also handle that. [https://codeforces.com/contest/763/submission/72618301](https://codeforces.com/contest/763/submission/72618301) After finding node x, y we're applying DFS on subtree to find if all elements there have same color or not. [https://codeforces.com/contest/763/submission/72618931](https://codeforces.com/contest/763/submission/72618931) Less intuitive approach of reducing DFS overhead
* [Greg and Graph](https://codeforces.com/contest/296/problem/D) - Given an adjancey matrix, and q queries specifying order to delete that vertex \(removing each of its edges\) Find sum of all pair distance every time before removing the vertex. **Solution:** Idea is similar to all pair shortest time algorithm - Floyd Warshall  [https://codeforces.com/contest/296/submission/74248854](https://codeforces.com/contest/296/submission/74248854)
* [Xenia and BIT Operations](https://codeforces.com/problemset/problem/339/D): Use the idea of expression tree that will reduce query time complexity to logN [https://codeforces.com/contest/339/submission/74685358](https://codeforces.com/contest/339/submission/74685358)
* [Tree Cutting \(Easy\)](https://codeforces.com/problemset/problem/1118/F1): Nice problem of showing power of DFS precomputation, simple precompute cnt \(when color in subtree is non zero\) sm \(when color in subtree is non zero\) and also depth. If we pick an edge we can check if it adds up to ans in O\(1\) by counting corresponding non-connected component sum/cnt \(this will give which color that component has, checking with % will make sure than no two different color exists. [https://codeforces.com/contest/1118/submission/74688957](https://codeforces.com/contest/1118/submission/74688957)
* [Swaps In Permutation](https://codeforces.com/contest/691/problem/D): Given a permutation array having elements 1 to n also given m pairs signifying swaps we can perform find such final permutation after performing swaps giving lexiographically largest permutation. **Solution:** If we form a graph based on the swaps and consider all the connected components, when first element of some component shows swap it with available largest from element within that component next time again then second largest and so on. [https://codeforces.com/contest/691/problem/D](https://codeforces.com/contest/691/problem/D)

## Flows & Cuts

![](.gitbook/assets/image%20%28110%29.png)

Given a directed weighted graph with source and sink, edges analogous to pipes, water comes from source and wents to sink. We want to find - Max Flow and Min Cut

![Maximum flow here is 7 \(out from source and in to sink\)](.gitbook/assets/image%20%28106%29.png)

In the minimum cut problem, our task is to remove a set of edges from the graph such that there will be no path from the source to the sink after the removal and the total weight of the removed edges is minimum.

![Minimum size of cut here is 7 \(remove 6 and 1\)](.gitbook/assets/image%20%28120%29.png)

Max Flow is always equal to min Cut. Why is the flow produced by the algorithm maximum and why is the cut minimum? The reason is that a graph cannot contain a flow whose size is larger than the weight of any cut of the graph. Hence, always when a flow and a cut are equally large, they are a maximum flow and a minimum cut.

Now the minimum cut consists of the edges of the original graph that start at some node in A, end at some node outside A, and whose capacity is fully used in the maximum flow.

### Ford-Fulkerson Algorithm

* It consists of several rounds, on each first find a path from source to sink.

![](.gitbook/assets/image%20%2889%29.png)

* Take minimum weight \(x\) of the edge from chosen path and subtract it through residual edges and add x to maxFlow count.

![](.gitbook/assets/image%20%2881%29.png)

Similarly

![](.gitbook/assets/image%20%2898%29.png)

![Still need 2 more rounds for optimized flow](.gitbook/assets/image%20%28109%29.png)

The idea is that increasing the flow decreases the amount of flow that can go through the edges in the future. On the other hand, it is possible to cancel flow later using the reverse edges of the graph if it turns out that it would be beneficial to route the flow in another way.

Now how to choose paths optimally within each ford-fulkerson round? We can use DFS but in worst case, each path only increases the flow by 1 and algorithm is slow. So we can use

### Edmonds-Karp Algorithm

Choose path with as small number of edges as possible. This can be done using BFS instead of DFS. It guarantees O\(m^2 n\) complexity

### Scaling Algorithm

Uses DFS to find paths where each edge weight is at least a threshold value. Initially, the threshold value is some large number, for example the sum of all edge weights of the graph. Always when a path cannot be found, the threshold value is divided by 2. The time complexity of the algorithm is O\(m^2 log c\) where c is initial threshold value.

### Dinic Algorithm

```cpp
struct FlowEdge
{
		int v, u;
		ll cap, flow = 0;
		FlowEdge(int v, int u, ll cap) : v(v), u(u), cap(cap) {}
};
struct Dinic
{
		const ll FLOW_INF = 1e18;
		vector<FlowEdge> edges;
		vector<vector<int>> adj;
		int n, m = 0;
		int s, t;
		vector<int> level, ptr;
		queue<int> q;
		Dinic(int n, int s, int t) : n(n), s(s), t(t)
		{
				adj.resize(n);
				level.resize(n);
				ptr.resize(n);
		}
		void addEdge(int u, int v, ll cap)
		{
				edges.emplace_back(u, v, cap);
				edges.emplace_back(v, u, 0);
				adj[u].push_back(m);
				adj[v].push_back(m + 1);
				m += 2;
		}
		bool bfs()
		{
				while (!q.empty())
				{
						int v = q.front();
						q.pop();
						for (int id : adj[v])
						{
								if (edges[id].cap - edges[id].flow < 1) continue;
								if (level[edges[id].u] != -1)	continue;
								level[edges[id].u] = level[v] + 1;
								q.push(edges[id].u);
						}
				}
				return level[t] != -1;
		}
		ll dfs(int v, ll pushed)
		{
				if (pushed == 0) return 0;
				if (v == t) return pushed;
				for (int &cid = ptr[v]; cid < (int)adj[v].size(); cid++)
				{
						int id = adj[v][cid], u = edges[id].u;
						if (level[v] + 1 != level[u] || edges[id].cap - edges[id].flow < 1)	continue;
						ll tr = dfs(u, min(pushed, edges[id].cap - edges[id].flow));
						if (tr == 0) continue;
						edges[id].flow += tr;
						edges[id ^ 1].flow -= tr;
						return tr;
				}
				return 0;
		}
		ll flow()
		{
				ll f = 0;
				while (true)
				{
						fill(level.begin(), level.end(), -1);
						level[s] = 0;
						q.push(s);
						if (!bfs()) break;
						fill(ptr.begin(), ptr.end(), 0);
						while (ll pushed = dfs(s, FLOW_INF)) f += pushed;
				}
				return f;
		}
};
// O(N^2 * E)
```

## Graph Matching - Hopcraft Korp Algorithm

```cpp
class GraphMatching
{
private:
    static bool findMatch(int i, const vector<vector<int>> &inp_w, vector<int> &out_row, vector<int> &out_column, vector<int> &seen)
    {
        // 0 is NO_EDGE here
        if (seen[i]) return false;
        seen[i] = true;
        for (int j = 0; j < inp_w[i].size(); ++j)
        {
            if (inp_w[i][j] != 0 && out_column[j] < 0)
            {
                out_row[i] = j, out_column[j] = i;
                return true;
            }
        }
        for (int j = 0; j < inp_w[i].size(); ++j)
        {
            if (inp_w[i][j] != 0 && out_row[i] != j)
            {
                if (out_column[j] < 0 || findMatch(out_column[j], inp_w, out_row, out_column, seen))
                {
                    out_row[i] = j, out_column[j] = i;
                    return true;
                }
            }
        }
        return false;
    }
    static bool dfs(int n, int N, vector<vector<int>> &inp_con, vector<int> &blossom, vector<int> &vis, vector<int> &out_match)
    {
        vis[n] = 0;
        for (int i = 0; i < N; ++i)
        {
            if (!inp_con[n][i]) continue;
            if (vis[i] == -1)
            {
                vis[i] = 1;
                if (out_match[i] == -1 || dfs(out_match[i], N, inp_con, blossom, vis, out_match))
                {
                    out_match[n] = i, out_match[i] = n;
                    return true;
                }
            }
            if (vis[i] == 0 || blossom.size())
            {
                blossom.push_back(i), blossom.push_back(n);
                if (n == blossom[0])
                {
                    out_match[n] = -1;
                    return true;
                }
                return false;
            }
        }
        return false;
    }
    static bool augment(int N, vector<vector<int>> &inp_con, vector<int> &out_match)
    {
        for (int i = 0; i < N; ++i)
        {
            if (out_match[i] != -1) continue;
            vector<int> blossom;
            vector<int> vis(N, -1);
            if (!dfs(i, N, inp_con, blossom, vis, out_match)) continue;
            if (blossom.size() == 0) return true;
            int base = blossom[0], S = blossom.size();
            vector<vector<int>> newCon = inp_con;
            for (int i = 1; i != S-1; ++i)
                for (int j = 0; j < N; ++j)
                    newCon[base][j] = newCon[j][base] |= inp_con[blossom[i]][j];
            for (int i = 1; i != S-1; ++i)
                for (int j = 0; j < N; ++j)
                    newCon[blossom[i]][j] = newCon[j][blossom[i]] = 0;
            newCon[base][base] = 0;
            if (!augment(N, newCon, out_match)) return false;
            int n = out_match[base];
            if (n != -1)
            {
                for (int i = 0; i < S; ++i)
                {
                    if (!inp_con[blossom[i]][n]) continue;
                    out_match[blossom[i]] = n, out_match[n] = blossom[i];
                    if (i&1)
                    {
                        for (int j = i+1; j < S; j += 2)
                            out_match[blossom[j]] = blossom[j+1], out_match[blossom[j+1]] = blossom[j];
                    }
                    else
                    {
                        for (int j = 0; j < i; j += 2)
                            out_match[blossom[j]] = blossom[j+1], out_match[blossom[j+1]] = blossom[j];
                    }
                    break;
                }
            }
            return true;
        }
        return false;
    }
public:
    /* weighted bipartite matching, inp_w[i][j] is cost from row node i to column node j
    out vector out_row & out_column is assignment for that node or -1 if unassigned
    It has a heuristic that will give excellent performance on complete graphs where rows <= columns. */
    static int bipartiteMatching(const vector<vector<int>> &inp_w, vector<int> &out_row, vector<int> &out_column)
    {
        out_row = vector<int> (inp_w.size(), -1);
        out_column = vector<int> (inp_w[0].size(), -1);
        vector<int> seen(inp_w.size());
        int count = 0;
        for (int i = 0; i < inp_w.size(); ++i)
        {
            fill(seen.begin(), seen.end(), 0);
            if (findMatch(i, inp_w, out_row, out_column, seen)) count++;
        }
        return count;
    }
    // unweighted inp_conn[i][j] = 1 for edge 0 otherwise, returns no. of edges in max matching
    static int nonBipartiteMatching(vector<vector<int>> &inp_con, vector<int> &out_match)
    {
        int res = 0;
        int N = inp_con.size();
        out_match = vector<int>(N, -1);
        while (augment(N, inp_con, out_match)) res++;
        return res;
    }
};
```

![](.gitbook/assets/image%20%28124%29.png)

```cpp
// Bipartite Matching
vector<vector<int>> inp_w = 
{
    {1, 1, 0, 0, 0},
    {1, 0, 0, 0, 0},
    {0, 1, 1, 0, 0},
    {0, 0, 1, 1, 1},
    {0, 0, 0, 0, 1},
};
vector<int> out_row, out_column;
int res = GraphMatching::bipartiteMatching(inp_w, out_row, out_column);
cout << res << endl;
for (int i : out_row) cout << i << " ";
cout << endl;
for (int i : out_column) cout << i << " ";
cout << endl;
```

![](.gitbook/assets/image%20%28129%29.png)

```cpp
// Non Bipartite Matching
vector<vector<int>> adj_mat =
{
    {0, 1, 1, 0, 0, 0, 0, 0},
    {0, 0, 0, 1, 1, 0, 0, 0},
    {1, 0, 0, 1, 0, 0, 0, 0},
    {0, 1, 1, 0, 1, 1, 0, 0},
    {0, 1, 0, 1, 0, 0, 0, 0},
    {0, 0, 0, 1, 0, 0, 1, 0},
    {0, 0, 0, 0, 0, 1, 0, 1},
    {0, 0, 0, 1, 0, 0, 1, 0}
};
vector<int> match;
int res = GraphMatching::nonBipartiteMatching(adj_mat, match);
cout << res << endl;
for (int i : match) cout << i << " ";
cout << endl;
```

## Graph Decomposition

TODO

