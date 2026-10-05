# 100 Top DSA Questions — Pattern-Wise Complete Revision Guide (C++)

> A long-form, interview-focused revision sheet covering 10 essential patterns. Each question includes the C++ solution, a clear explanation, an example, and 4–5 key takeaways on the pattern, edge cases, and time/space complexity.

---

## Table of Contents

1. [Linked List](#1-linked-list)
2. [Stack and Queue](#2-stack-and-queue)
3. [Graphs and Trees](#3-graphs-and-trees)
4. [Dynamic Programming](#4-dynamic-programming)
5. [Recursion and Backtracking](#5-recursion-and-backtracking)
6. [Prefix Sum and HashSets](#6-prefix-sum-and-hashsets)
7. [Sliding Window and HashMap](#7-sliding-window-and-hashmap)
8. [Searching and Sorting](#8-searching-and-sorting)
9. [Two Pointers](#9-two-pointers)
10. [Greedy and Bit Manipulation](#10-greedy-and-bit-manipulation)

---

## 1. Linked List

### Q1. Reverse a Linked List (LC 206)

**Problem:** Reverse a singly linked list.

**C++ Solution (Iterative):**
```cpp
ListNode* reverseList(ListNode* head) {
    ListNode *prev = nullptr, *curr = head;
    while (curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

**Example:** `1 → 2 → 3 → NULL` becomes `3 → 2 → 1 → NULL`.

**Key Points:**
1. **Pattern:** Three-pointer manipulation (`prev`, `curr`, `next`).
2. **Time:** O(n) — single pass. **Space:** O(1) — iterative.
3. Recursive version uses O(n) stack space; iterative is preferred.
4. Edge cases: empty list, single node.
5. Foundation for many linked-list problems (reverse in groups, palindrome check).

---

### Q2. Detect Cycle in a Linked List (LC 141)

**Problem:** Return `true` if the linked list has a cycle.

**C++ Solution (Floyd’s Cycle Detection):**
```cpp
bool hasCycle(ListNode *head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

**Example:** `3 → 2 → 0 → -4 → (back to 2)` → `true`.

**Key Points:**
1. **Pattern:** Fast & slow pointers (tortoise and hare).
2. **Time:** O(n). **Space:** O(1).
3. If there is a cycle, fast will eventually meet slow.
4. To find the cycle start, reset one pointer to head after meeting.
5. Also used in finding the middle of a list and palindrome checking.

---

### Q3. Merge Two Sorted Lists (LC 21)

**Problem:** Merge two sorted linked lists into one sorted list.

**C++ Solution (Dummy Node):**
```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* tail = &dummy;
    while (l1 && l2) {
        if (l1->val < l2->val) { tail->next = l1; l1 = l1->next; }
        else { tail->next = l2; l2 = l2->next; }
        tail = tail->next;
    }
    tail->next = l1 ? l1 : l2;
    return dummy.next;
}
```

**Example:** `1→2→4` and `1→3→4` → `1→1→2→3→4→4`.

**Key Points:**
1. **Pattern:** Dummy node simplifies edge cases (empty lists, head changes).
2. **Time:** O(n+m). **Space:** O(1).
3. Used as a subroutine in merge sort on linked lists.
4. Handles unequal lengths by appending the remainder.
5. Alternative: recursive merge (O(n+m) stack space).

---

### Q4. Remove Nth Node From End of List (LC 19)

**Problem:** Remove the n-th node from the end.

**C++ Solution (Two-Pass with Gap):**
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0); dummy.next = head;
    ListNode *fast = &dummy, *slow = &dummy;
    for (int i = 0; i <= n; i++) fast = fast->next;
    while (fast) { slow = slow->next; fast = fast->next; }
    slow->next = slow->next->next;
    return dummy.next;
}
```

**Example:** `1→2→3→4→5`, n=2 → `1→2→3→5`.

**Key Points:**
1. **Pattern:** Two pointers with a gap of n+1 (using dummy).
2. **Time:** O(L). **Space:** O(1).
3. Dummy node avoids separate handling when removing the head.
4. Works in one pass after the initial gap.
5. Similar technique for finding the middle or k-th from end.

---

### Q5. Palindrome Linked List (LC 234)

**Problem:** Check if a linked list is a palindrome.

**C++ Solution (Find Middle + Reverse + Compare):**
```cpp
bool isPalindrome(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) { slow = slow->next; fast = fast->next->next; }
    ListNode *prev = nullptr;
    while (slow) { ListNode* nxt = slow->next; slow->next = prev; prev = slow; slow = nxt; }
    ListNode *left = head, *right = prev;
    while (right) { if (left->val != right->val) return false; left = left->next; right = right->next; }
    return true;
}
```

**Example:** `1→2→2→1` → `true`.

**Key Points:**
1. **Pattern:** Fast/slow pointer + reverse half + compare.
2. **Time:** O(n). **Space:** O(1).
3. Restoring the list is optional but good practice.
4. Edge cases: odd length (middle node ignored).
5. Combines three fundamental linked-list operations.

---

### Q6. Intersection of Two Linked Lists (LC 160)

**Problem:** Find the node where two lists intersect.

**C++ Solution (Length Difference):**
```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    int lenA = 0, lenB = 0;
    for (ListNode* p = headA; p; p = p->next) lenA++;
    for (ListNode* p = headB; p; p = p->next) lenB++;
    ListNode *a = headA, *b = headB;
    while (lenA > lenB) { a = a->next; lenA--; }
    while (lenB > lenA) { b = b->next; lenB--; }
    while (a != b) { a = a->next; b = b->next; }
    return a;
}
```

**Example:** Two lists sharing `8→4→5`; return node with value 8.

**Key Points:**
1. **Pattern:** Align starting points by skipping the longer list’s extra nodes.
2. **Time:** O(m+n). **Space:** O(1).
3. Intersection is by node reference, not value.
4. Alternative two-pointer switch trick (O(m+n), O(1)).
5. Edge cases: no intersection, one list empty.

---

### Q7. Add Two Numbers (LC 2)

**Problem:** Add two numbers represented by linked lists in reverse order.

**C++ Solution:**
```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode dummy(0); ListNode* tail = &dummy;
    int carry = 0;
    while (l1 || l2 || carry) {
        int sum = carry;
        if (l1) { sum += l1->val; l1 = l1->next; }
        if (l2) { sum += l2->val; l2 = l2->next; }
        carry = sum / 10;
        tail->next = new ListNode(sum % 10);
        tail = tail->next;
    }
    return dummy.next;
}
```

**Example:** `(2→4→3) + (5→6→4)` = `7→0→8` (342 + 465 = 807).

**Key Points:**
1. **Pattern:** Simultaneous traversal with carry.
2. **Time:** O(max(m,n)). **Space:** O(max(m,n)) for the result.
3. Dummy node simplifies result construction.
4. Handles different lengths and final carry.
5. If digits are forward, reverse lists first or use stacks.

---

### Q8. Copy List with Random Pointer (LC 138)

**Problem:** Deep copy a list where each node has a `next` and a `random` pointer.

**C++ Solution (Hash Map):**
```cpp
Node* copyRandomList(Node* head) {
    if (!head) return nullptr;
    unordered_map<Node*, Node*> m;
    Node* curr = head;
    while (curr) { m[curr] = new Node(curr->val); curr = curr->next; }
    curr = head;
    while (curr) {
        m[curr]->next = m[curr->next];
        m[curr]->random = m[curr->random];
        curr = curr->next;
    }
    return m[head];
}
```

**Example:** Original: `7→13→11`; copy has same structure with independent nodes.

**Key Points:**
1. **Pattern:** Two-pass with hash map (old → new).
2. **Time:** O(n). **Space:** O(n).
3. Interleaving method achieves O(1) extra space.
4. Must copy `random` pointers even if they point forward/backward.
5. Common in system design / cloning graphs.

---

### Q9. Reorder List (LC 143)

**Problem:** Reorder `L0→L1→…→Ln` to `L0→Ln→L1→Ln-1→…`.

**C++ Solution (Find Middle, Reverse Second Half, Merge):**
```cpp
void reorderList(ListNode* head) {
    if (!head || !head->next) return;
    ListNode *slow = head, *fast = head;
    while (fast->next && fast->next->next) { slow = slow->next; fast = fast->next->next; }
    ListNode *prev = nullptr, *curr = slow->next;
    while (curr) { ListNode* nxt = curr->next; curr->next = prev; prev = curr; curr = nxt; }
    slow->next = nullptr;
    ListNode *first = head, *second = prev;
    while (second) {
        ListNode *tmp1 = first->next, *tmp2 = second->next;
        first->next = second; second->next = tmp1;
        first = tmp1; second = tmp2;
    }
}
```

**Example:** `1→2→3→4` → `1→4→2→3`.

**Key Points:**
1. **Pattern:** Middle + reverse + merge interleaving.
2. **Time:** O(n). **Space:** O(1).
3. Combines three classic linked-list techniques.
4. Careful pointer updates to avoid losing nodes.
5. Edge cases: 0, 1, 2 nodes.

---

### Q10. Merge K Sorted Lists (LC 23)

**Problem:** Merge k sorted linked lists.

**C++ Solution (Min-Heap):**
```cpp
struct cmp { bool operator()(ListNode* a, ListNode* b){ return a->val > b->val; } };
ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<ListNode*, vector<ListNode*>, cmp> pq;
    for (auto l : lists) if (l) pq.push(l);
    ListNode dummy(0); ListNode* tail = &dummy;
    while (!pq.empty()) {
        ListNode* node = pq.top(); pq.pop();
        tail->next = node; tail = tail->next;
        if (node->next) pq.push(node->next);
    }
    return dummy.next;
}
```

**Example:** `[[1,4,5],[1,3,4],[2,6]]` → `1→1→2→3→4→4→5→6`.

**Key Points:**
1. **Pattern:** Min-heap of list heads.
2. **Time:** O(N log k), N = total nodes. **Space:** O(k).
3. Alternative: divide and conquer (O(N log k), O(1) space).
4. Heap keeps the next smallest node ready.
5. Scales well for large k.

---

## 2. Stack and Queue

### Q1. Valid Parentheses (LC 20)

**Problem:** Check if brackets are balanced.

**C++ Solution:**
```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') st.push(c);
        else {
            if (st.empty()) return false;
            char t = st.top(); st.pop();
            if ((c == ')' && t != '(') || (c == '}' && t != '{') || (c == ']' && t != '['))
                return false;
        }
    }
    return st.empty();
}
```

**Example:** `"()[]{}"` → `true`; `"(]"` → `false`.

**Key Points:**
1. **Pattern:** Stack for matching pairs.
2. **Time:** O(n). **Space:** O(n).
3. Early exit on mismatch.
4. Check empty stack before popping.
5. Extended to multiple bracket types and score calculation.

---

### Q2. Min Stack (LC 155)

**Problem:** Design a stack supporting push, pop, top, and getMin in O(1).

**C++ Solution (Two Stacks):**
```cpp
class MinStack {
    stack<int> s, minS;
public:
    void push(int x) {
        s.push(x);
        if (minS.empty() || x <= minS.top()) minS.push(x);
    }
    void pop() {
        if (s.top() == minS.top()) minS.pop();
        s.pop();
    }
    int top() { return s.top(); }
    int getMin() { return minS.top(); }
};
```

**Example:** push(-2), push(0), push(-3) → getMin() = -3.

**Key Points:**
1. **Pattern:** Auxiliary stack tracking current minimum.
2. All operations O(1). **Space:** O(n).
3. Use `<=` in push to handle duplicates.
4. Alternative: store (value, min) pairs in one stack.
5. Common follow-up: Max Stack.

---

### Q3. Implement Queue using Stacks (LC 232)

**Problem:** Implement FIFO queue using two LIFO stacks.

**C++ Solution (Amortized O(1)):**
```cpp
class MyQueue {
    stack<int> in, out;
    void transfer() {
        if (out.empty())
            while (!in.empty()) { out.push(in.top()); in.pop(); }
    }
public:
    void push(int x) { in.push(x); }
    int pop() { transfer(); int v = out.top(); out.pop(); return v; }
    int peek() { transfer(); return out.top(); }
    bool empty() { return in.empty() && out.empty(); }
};
```

**Example:** push(1), push(2), pop() → 1, peek() → 2.

**Key Points:**
1. **Pattern:** Two stacks for queue semantics.
2. Amortized O(1) per operation. **Space:** O(n).
3. Transfer only when `out` is empty.
4. Avoids O(n) pop by lazy transfer.
5. Symmetric: Implement Stack using Queues.

---

### Q4. Next Greater Element I (LC 496)

**Problem:** For each element in `nums1`, find the next greater element in `nums2`.

**C++ Solution (Monotonic Stack + Hash Map):**
```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    unordered_map<int,int> nge;
    stack<int> st;
    for (int x : nums2) {
        while (!st.empty() && st.top() < x) { nge[st.top()] = x; st.pop(); }
        st.push(x);
    }
    vector<int> res;
    for (int x : nums1) res.push_back(nge.count(x) ? nge[x] : -1);
    return res;
}
```

**Example:** `nums1=[4,1,2]`, `nums2=[1,3,4,2]` → `[-1,3,-1]`.

**Key Points:**
1. **Pattern:** Monotonic decreasing stack.
2. **Time:** O(n+m). **Space:** O(n).
3. Elements without a greater next remain in stack → -1.
4. Generalizes to Next Greater Element II (circular).
5. Core technique for daily temperatures, stock span, largest rectangle.

---

### Q5. Daily Temperatures (LC 739)

**Problem:** For each day, how many days until a warmer temperature?

**C++ Solution (Monotonic Stack of Indices):**
```cpp
vector<int> dailyTemperatures(vector<int>& T) {
    int n = T.size(); vector<int> res(n, 0);
    stack<int> st;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && T[i] > T[st.top()]) {
            int idx = st.top(); st.pop();
            res[idx] = i - idx;
        }
        st.push(i);
    }
    return res;
}
```

**Example:** `[73,74,75,71,69,72,76,73]` → `[1,1,4,2,1,1,0,0]`.

**Key Points:**
1. **Pattern:** Monotonic increasing stack of indices.
2. **Time:** O(n). **Space:** O(n).
3. Maintains unresolved days; current warmer day resolves them.
4. Classic “next greater element” variant.
5. Can be adapted for stock span (previous greater).

---

### Q6. Evaluate Reverse Polish Notation (LC 150)

**Problem:** Evaluate an expression in Reverse Polish Notation.

**C++ Solution:**
```cpp
int evalRPN(vector<string>& tokens) {
    stack<int> st;
    for (string& t : tokens) {
        if (t == "+" || t == "-" || t == "*" || t == "/") {
            int b = st.top(); st.pop();
            int a = st.top(); st.pop();
            if (t == "+") st.push(a + b);
            else if (t == "-") st.push(a - b);
            else if (t == "*") st.push(a * b);
            else st.push(a / b);
        } else st.push(stoi(t));
    }
    return st.top();
}
```

**Example:** `["2","1","+","3","*"]` → `(2+1)*3 = 9`.

**Key Points:**
1. **Pattern:** Stack for operands.
2. **Time:** O(n). **Space:** O(n).
3. Operand order matters for `-` and `/`.
4. Division truncates toward zero in C++.
5. Foundation for expression parsing.

---

### Q7. Sliding Window Maximum (LC 239)

**Problem:** Return the maximum in every sliding window of size `k`.

**C++ Solution (Monotonic Deque):**
```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq; vector<int> res;
    for (int i = 0; i < nums.size(); i++) {
        while (!dq.empty() && dq.front() <= i - k) dq.pop_front();
        while (!dq.empty() && nums[dq.back()] <= nums[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) res.push_back(nums[dq.front()]);
    }
    return res;
}
```

**Example:** `[1,3,-1,-3,5,3,6,7]`, k=3 → `[3,3,5,5,6,7]`.

**Key Points:**
1. **Pattern:** Monotonic deque (decreasing).
2. **Time:** O(n). **Space:** O(k).
3. Deque front is the maximum of current window.
4. Remove out-of-window indices from front.
5. Used in stock analysis, sliding window median variants.

---

### Q8. Largest Rectangle in Histogram (LC 84)

**Problem:** Find the largest rectangle area in a histogram.

**C++ Solution (Monotonic Stack):**
```cpp
int largestRectangleArea(vector<int>& h) {
    h.push_back(0); stack<int> st; int maxA = 0;
    for (int i = 0; i < h.size(); i++) {
        while (!st.empty() && h[st.top()] > h[i]) {
            int height = h[st.top()]; st.pop();
            int width = st.empty() ? i : i - st.top() - 1;
            maxA = max(maxA, height * width);
        }
        st.push(i);
    }
    return maxA;
}
```

**Example:** `[2,1,5,6,2,3]` → `10`.

**Key Points:**
1. **Pattern:** Monotonic increasing stack.
2. **Time:** O(n). **Space:** O(n).
3. Sentinel `0` flushes remaining bars.
4. Width derived from previous smaller and current index.
5. Extends to Maximal Rectangle in binary matrix.

---

### Q9. Implement Stack using Queues (LC 225)

**Problem:** Implement LIFO stack using queues.

**C++ Solution (One Queue, Rotate on Push):**
```cpp
class MyStack {
    queue<int> q;
public:
    void push(int x) {
        q.push(x);
        for (int i = 0; i < q.size() - 1; i++) {
            q.push(q.front()); q.pop();
        }
    }
    int pop() { int v = q.front(); q.pop(); return v; }
    int top() { return q.front(); }
    bool empty() { return q.empty(); }
};
```

**Example:** push(1), push(2), top() → 2, pop() → 2.

**Key Points:**
1. **Pattern:** Single queue with rotation.
2. Push: O(n), pop/top: O(1). **Space:** O(n).
3. Rotating places newest element at front.
4. Alternative: two queues (O(n) push or O(n) pop).
5. Classic “adapter” design question.

---

### Q10. Car Fleet (LC 853)

**Problem:** Count how many car fleets arrive at a destination.

**C++ Solution (Sort by Position, Monotonic Stack):**
```cpp
int carFleet(int target, vector<int>& pos, vector<int>& speed) {
    int n = pos.size(); vector<pair<int,double>> cars;
    for (int i = 0; i < n; i++)
        cars.push_back({pos[i], (double)(target - pos[i]) / speed[i]});
    sort(cars.rbegin(), cars.rend());
    int fleets = 0; double lastTime = 0;
    for (auto& [p, t] : cars) {
        if (t > lastTime) { fleets++; lastTime = t; }
    }
    return fleets;
}
```

**Example:** target=12, pos=[10,8,0,5,3], speed=[2,4,1,1,3] → 3.

**Key Points:**
1. **Pattern:** Sort by position + stack of arrival times.
2. **Time:** O(n log n). **Space:** O(n).
3. A car catches up if its arrival time ≤ fleet ahead.
4. Only fleets with strictly increasing times count.
5. Greedy + sorting + monotonic logic.

---

## 3. Graphs and Trees

### Q1. Number of Islands (LC 200)

**Problem:** Count islands in a 2D grid (1 = land, 0 = water).

**C++ Solution (DFS Flood Fill):**
```cpp
void dfs(vector<vector<char>>& g, int i, int j) {
    if (i<0||j<0||i>=g.size()||j>=g[0].size()||g[i][j]!='1') return;
    g[i][j] = '0';
    dfs(g,i+1,j); dfs(g,i-1,j); dfs(g,i,j+1); dfs(g,i,j-1);
}
int numIslands(vector<vector<char>>& grid) {
    int count = 0;
    for (int i=0;i<grid.size();i++)
        for (int j=0;j<grid[0].size();j++)
            if (grid[i][j]=='1') { dfs(grid,i,j); count++; }
    return count;
}
```

**Example:** 4 islands in a 5×5 grid.

**Key Points:**
1. **Pattern:** DFS/BFS flood fill on implicit graph.
2. **Time:** O(mn). **Space:** O(mn) recursion worst case.
3. Mark visited by mutating grid (or use visited array).
4. BFS avoids stack overflow for large grids.
5. Extends to perimeter, max area, number of distinct islands.

---

### Q2. Clone Graph (LC 133)

**Problem:** Deep copy an undirected graph.

**C++ Solution (DFS + Hash Map):**
```cpp
unordered_map<Node*, Node*> mp;
Node* cloneGraph(Node* node) {
    if (!node) return nullptr;
    if (mp.count(node)) return mp[node];
    Node* copy = new Node(node->val);
    mp[node] = copy;
    for (Node* nei : node->neighbors)
        copy->neighbors.push_back(cloneGraph(nei));
    return copy;
}
```

**Example:** Clone a graph with nodes 1–4 and edges as given.

**Key Points:**
1. **Pattern:** DFS with memoization (old → new).
2. **Time:** O(V+E). **Space:** O(V).
3. Map prevents infinite recursion on cycles.
4. BFS version uses queue + map.
5. Foundation for graph traversal and copy problems.

---

### Q3. Course Schedule (LC 207)

**Problem:** Determine if all courses can be finished (cycle detection in directed graph).

**C++ Solution (Kahn’s Topological Sort):**
```cpp
bool canFinish(int n, vector<vector<int>>& pre) {
    vector<vector<int>> adj(n); vector<int> indeg(n,0);
    for (auto& p : pre) { adj[p[1]].push_back(p[0]); indeg[p[0]]++; }
    queue<int> q;
    for (int i=0;i<n;i++) if (indeg[i]==0) q.push(i);
    int cnt = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop(); cnt++;
        for (int v : adj[u]) if (--indeg[v]==0) q.push(v);
    }
    return cnt == n;
}
```

**Example:** `numCourses=2, prerequisites=[[1,0]]` → `true`.

**Key Points:**
1. **Pattern:** Topological sort (BFS + indegree).
2. **Time:** O(V+E). **Space:** O(V+E).
3. Cycle exists iff not all nodes are processed.
4. DFS with recursion stack also detects cycles.
5. Core for scheduling, dependency resolution.

---

### Q4. Pacific Atlantic Water Flow (LC 417)

**Problem:** Find cells from which water can flow to both oceans.

**C++ Solution (Reverse DFS from Both Oceans):**
```cpp
void dfs(vector<vector<int>>& h, vector<vector<bool>>& vis, int i, int j) {
    vis[i][j] = true;
    int dirs[4][2] = {{1,0},{-1,0},{0,1},{0,-1}};
    for (auto& d : dirs) {
        int ni = i+d[0], nj = j+d[1];
        if (ni>=0 && nj>=0 && ni<h.size() && nj<h[0].size()
            && !vis[ni][nj] && h[ni][nj] >= h[i][j])
            dfs(h, vis, ni, nj);
    }
}
vector<vector<int>> pacificAtlantic(vector<vector<int>>& h) {
    int m=h.size(), n=h[0].size();
    vector<vector<bool>> p(m, vector<bool>(n)), a(m, vector<bool>(n));
    for (int i=0;i<m;i++) { dfs(h,p,i,0); dfs(h,a,i,n-1); }
    for (int j=0;j<n;j++) { dfs(h,p,0,j); dfs(h,a,m-1,j); }
    vector<vector<int>> res;
    for (int i=0;i<m;i++) for (int j=0;j<n;j++) if (p[i][j]&&a[i][j]) res.push_back({i,j});
    return res;
}
```

**Example:** Return cells reachable from both top-left and bottom-right edges.

**Key Points:**
1. **Pattern:** Reverse DFS from ocean borders.
2. **Time:** O(mn). **Space:** O(mn).
3. Flow direction reversed: water flows from lower/equal to higher.
4. Two visited matrices for Pacific and Atlantic.
5. Similar to “Surrounded Regions.”

---

### Q5. Binary Tree Level Order Traversal (LC 102)

**Problem:** Return level-by-level traversal of a binary tree.

**C++ Solution (BFS with Queue):**
```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (!root) return res;
    queue<TreeNode*> q; q.push(root);
    while (!q.empty()) {
        int sz = q.size(); vector<int> level;
        while (sz--) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        res.push_back(level);
    }
    return res;
}
```

**Example:** `[3,9,20,null,null,15,7]` → `[[3],[9,20],[15,7]]`.

**Key Points:**
1. **Pattern:** BFS with level-size snapshot.
2. **Time:** O(n). **Space:** O(n) worst case.
3. Foundation for zigzag, right-side view, max width.
4. Can use DFS with depth parameter.
5. Queue is the standard BFS tool.

---

### Q6. Validate Binary Search Tree (LC 98)

**Problem:** Check if a binary tree is a valid BST.

**C++ Solution (Inorder Traversal):**
```cpp
bool isValidBST(TreeNode* root) {
    TreeNode* prev = nullptr;
    function<bool(TreeNode*)> inorder = [&](TreeNode* node) {
        if (!node) return true;
        if (!inorder(node->left)) return false;
        if (prev && prev->val >= node->val) return false;
        prev = node;
        return inorder(node->right);
    };
    return inorder(root);
}
```

**Example:** `[2,1,3]` → `true`; `[5,1,4,null,null,3,6]` → `false`.

**Key Points:**
1. **Pattern:** Inorder traversal yields sorted order for BST.
2. **Time:** O(n). **Space:** O(h).
3. Track previous node to detect violations.
4. Alternative: min/max range propagation.
5. Strict inequality required (no duplicates).

---

### Q7. Lowest Common Ancestor of a Binary Tree (LC 236)

**Problem:** Find LCA of two nodes in a binary tree.

**C++ Solution (Recursive DFS):**
```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    return left && right ? root : (left ? left : right);
}
```

**Example:** LCA of 5 and 1 in `[3,5,1,6,2,0,8]` is 3.

**Key Points:**
1. **Pattern:** Post-order DFS.
2. **Time:** O(n). **Space:** O(h).
3. If both subtrees return non-null, current is LCA.
4. For BST, can use value comparison for O(h).
5. Foundation for distance between nodes.

---

### Q8. Diameter of Binary Tree (LC 543)

**Problem:** Find the longest path between any two nodes.

**C++ Solution (DFS with Height):**
```cpp
int diameterOfBinaryTree(TreeNode* root) {
    int dia = 0;
    function<int(TreeNode*)> height = [&](TreeNode* node) {
        if (!node) return 0;
        int l = height(node->left), r = height(node->right);
        dia = max(dia, l + r);
        return 1 + max(l, r);
    };
    height(root);
    return dia;
}
```

**Example:** `[1,2,3,4,5]` → 3 (path 4-2-1-3 or 5-2-1-3).

**Key Points:**
1. **Pattern:** Post-order DFS tracking height and diameter.
2. **Time:** O(n). **Space:** O(h).
3. Diameter through a node = left height + right height.
4. Can combine with max path sum logic.
5. Similar to tree DP problems.

---

### Q9. Word Ladder (LC 127)

**Problem:** Shortest transformation sequence from `beginWord` to `endWord`.

**C++ Solution (BFS on Word Graph):**
```cpp
int ladderLength(string begin, string end, vector<string>& wordList) {
    unordered_set<string> dict(wordList.begin(), wordList.end());
    if (!dict.count(end)) return 0;
    queue<string> q; q.push(begin); int steps = 1;
    while (!q.empty()) {
        int sz = q.size();
        while (sz--) {
            string word = q.front(); q.pop();
            if (word == end) return steps;
            for (int i = 0; i < word.size(); i++) {
                char orig = word[i];
                for (char c = 'a'; c <= 'z'; c++) {
                    word[i] = c;
                    if (dict.count(word)) {
                        q.push(word); dict.erase(word);
                    }
                }
                word[i] = orig;
            }
        }
        steps++;
    }
    return 0;
}
```

**Example:** `hit → hot → dot → dog → cog` → 5 steps.

**Key Points:**
1. **Pattern:** BFS on implicit graph of words.
2. **Time:** O(N * L * 26). **Space:** O(N).
3. Erase from set when enqueued to avoid revisits.
4. Bidirectional BFS optimizes further.
5. Tests graph modeling and BFS.

---

### Q10. Network Delay Time (LC 743)

**Problem:** Find time for all nodes to receive a signal (Dijkstra).

**C++ Solution (Dijkstra with Min-Heap):**
```cpp
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    vector<vector<pair<int,int>>> adj(n+1);
    for (auto& t : times) adj[t[0]].push_back({t[1], t[2]});
    vector<int> dist(n+1, INT_MAX); dist[k] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, k});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    int maxD = 0;
    for (int i=1;i<=n;i++) { if (dist[i]==INT_MAX) return -1; maxD = max(maxD, dist[i]); }
    return maxD;
}
```

**Example:** `times=[[2,1,1],[2,3,1],[3,4,1]], n=4, k=2` → 2.

**Key Points:**
1. **Pattern:** Dijkstra’s shortest path.
2. **Time:** O(E log V). **Space:** O(V+E).
3. Min-heap extracts closest unvisited node.
4. Relax edges and update distances.
5. Foundation for weighted graph shortest paths.

---

## 4. Dynamic Programming

### Q1. Climbing Stairs (LC 70)

**Problem:** Count ways to climb n stairs (1 or 2 steps).

**C++ Solution (Space-Optimized DP):**
```cpp
int climbStairs(int n) {
    int a = 1, b = 1;
    for (int i = 2; i <= n; i++) { int c = a + b; a = b; b = c; }
    return b;
}
```

**Example:** n=3 → 3 ways (1+1+1, 1+2, 2+1).

**Key Points:**
1. **Pattern:** Fibonacci-like 1D DP.
2. **Time:** O(n). **Space:** O(1).
3. Recurrence: `dp[i] = dp[i-1] + dp[i-2]`.
4. Can be solved with matrix exponentiation O(log n).
5. Gateway to more complex DP.

---

### Q2. House Robber (LC 198)

**Problem:** Max amount robbed without robbing adjacent houses.

**C++ Solution:**
```cpp
int rob(vector<int>& nums) {
    int prev = 0, curr = 0;
    for (int x : nums) { int tmp = max(curr, prev + x); prev = curr; curr = tmp; }
    return curr;
}
```

**Example:** `[2,7,9,3,1]` → 12 (2+9+1).

**Key Points:**
1. **Pattern:** 1D DP with two variables.
2. **Time:** O(n). **Space:** O(1).
3. Choice: rob current + i-2, or skip.
4. Circular version (LC 213) splits into two cases.
5. Also solvable with DP array for clarity.

---

### Q3. Coin Change (LC 322)

**Problem:** Fewest coins to make amount (unbounded knapsack).

**C++ Solution (Bottom-Up DP):**
```cpp
int coinChange(vector<int>& coins, int amount) {
    vector<int> dp(amount+1, amount+1); dp[0] = 0;
    for (int i = 1; i <= amount; i++)
        for (int c : coins)
            if (c <= i) dp[i] = min(dp[i], dp[i-c] + 1);
    return dp[amount] > amount ? -1 : dp[amount];
}
```

**Example:** `coins=[1,2,5], amount=11` → 3 (5+5+1).

**Key Points:**
1. **Pattern:** Unbounded knapsack / shortest path on amount.
2. **Time:** O(amount * coins). **Space:** O(amount).
3. `dp[i]` = min coins to make i.
4. Initialize with amount+1 (impossible).
5. Foundation for many coin problems.

---

### Q4. Longest Increasing Subsequence (LC 300)

**Problem:** Length of longest strictly increasing subsequence.

**C++ Solution (Patience Sorting – O(n log n)):**
```cpp
int lengthOfLIS(vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}
```

**Example:** `[10,9,2,5,3,7,101,18]` → 4 (2,3,7,101).

**Key Points:**
1. **Pattern:** DP + binary search (patience sorting).
2. **Time:** O(n log n). **Space:** O(n).
3. `tails[i]` = smallest tail of increasing subsequence of length i+1.
4. O(n²) DP is simpler but slower.
5. Reconstruct subsequence with parent pointers.

---

### Q5. Longest Common Subsequence (LC 1143)

**Problem:** Length of LCS of two strings.

**C++ Solution (2D DP):**
```cpp
int longestCommonSubsequence(string a, string b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1));
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = a[i-1] == b[j-1] ? dp[i-1][j-1] + 1 : max(dp[i-1][j], dp[i][j-1]);
    return dp[m][n];
}
```

**Example:** `"abcde"`, `"ace"` → 3 ("ace").

**Key Points:**
1. **Pattern:** 2D DP on prefixes.
2. **Time:** O(mn). **Space:** O(mn), O(n) with rolling array.
3. Match → diagonal + 1; else max of skip.
4. Used in diff, edit distance, DNA alignment.
5. Subsequence ≠ substring (no contiguity).

---

### Q6. 0/1 Knapsack (Classic)

**Problem:** Max value with weight limit W, each item used once.

**C++ Solution (1D DP):**
```cpp
int knapsack(vector<int>& wt, vector<int>& val, int W) {
    int n = wt.size(); vector<int> dp(W+1, 0);
    for (int i = 0; i < n; i++)
        for (int w = W; w >= wt[i]; w--)
            dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
    return dp[W];
}
```

**Example:** `wt=[1,3,4,5], val=[1,4,5,7], W=7` → 9.

**Key Points:**
1. **Pattern:** 0/1 knapsack DP (reverse iteration).
2. **Time:** O(nW). **Space:** O(W).
3. Reverse prevents using item multiple times.
4. Unbounded knapsack iterates forward.
5. Core for subset sum, partition, target sum.

---

### Q7. Edit Distance (LC 72)

**Problem:** Min operations (insert, delete, replace) to convert word1 to word2.

**C++ Solution:**
```cpp
int minDistance(string a, string b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m+1, vector<int>(n+1));
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = a[i-1] == b[j-1] ? dp[i-1][j-1]
                      : 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
    return dp[m][n];
}
```

**Example:** `"horse"`, `"ros"` → 3.

**Key Points:**
1. **Pattern:** 2D DP on prefixes.
2. **Time:** O(mn). **Space:** O(mn), O(n) with rolling.
3. Three operations map to three directions.
4. Base cases: empty string requires length ops.
5. Used in spell check, DNA alignment.

---

### Q8. Partition Equal Subset Sum (LC 416)

**Problem:** Can the array be partitioned into two equal-sum subsets?

**C++ Solution (1D DP – Subset Sum):**
```cpp
bool canPartition(vector<int>& nums) {
    int sum = accumulate(nums.begin(), nums.end(), 0);
    if (sum % 2) return false;
    int target = sum / 2;
    vector<bool> dp(target+1, false); dp[0] = true;
    for (int x : nums)
        for (int w = target; w >= x; w--)
            dp[w] = dp[w] || dp[w - x];
    return dp[target];
}
```

**Example:** `[1,5,11,5]` → `true` (11 vs 1+5+5).

**Key Points:**
1. **Pattern:** Subset sum → 0/1 knapsack.
2. **Time:** O(n * target). **Space:** O(target).
3. Target = total sum / 2.
4. Reverse iteration for 0/1.
5. Foundation for partition problems.

---

### Q9. Maximum Subarray (Kadane’s) (LC 53)

**Problem:** Find the contiguous subarray with the largest sum.

**C++ Solution:**
```cpp
int maxSubArray(vector<int>& nums) {
    int best = nums[0], curr = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        curr = max(nums[i], curr + nums[i]);
        best = max(best, curr);
    }
    return best;
}
```

**Example:** `[-2,1,-3,4,-1,2,1,-5,4]` → 6 (`[4,-1,2,1]`).

**Key Points:**
1. **Pattern:** Kadane’s algorithm (1D DP).
2. **Time:** O(n). **Space:** O(1).
3. `curr` = max sum ending at i.
4. Reset when `nums[i] > curr + nums[i]`.
5. Handles all-negative arrays.

---

### Q10. Unique Paths (LC 62)

**Problem:** Count paths from top-left to bottom-right in an m×n grid (right/down).

**C++ Solution (1D DP):**
```cpp
int uniquePaths(int m, int n) {
    vector<int> dp(n, 1);
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[j] += dp[j-1];
    return dp[n-1];
}
```

**Example:** m=3, n=7 → 28.

**Key Points:**
1. **Pattern:** 2D DP compressed to 1D.
2. **Time:** O(mn). **Space:** O(n).
3. `dp[j] = dp[j] + dp[j-1]` (from top + left).
4. Obstacles version sets dp[j]=0.
5. Combinatorial solution: C(m+n-2, m-1).

---

## 5. Recursion and Backtracking

### Q1. Subsets (LC 78)

**Problem:** Generate all subsets of a set of distinct integers.

**C++ Solution (Backtracking):**
```cpp
void backtrack(vector<int>& nums, int start, vector<int>& path, vector<vector<int>>& res) {
    res.push_back(path);
    for (int i = start; i < nums.size(); i++) {
        path.push_back(nums[i]);
        backtrack(nums, i+1, path, res);
        path.pop_back();
    }
}
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> res; vector<int> path;
    backtrack(nums, 0, path, res);
    return res;
}
```

**Example:** `[1,2,3]` → `[[],[1],[2],[3],[1,2],[1,3],[2,3],[1,2,3]]`.

**Key Points:**
1. **Pattern:** Backtracking with start index.
2. **Time:** O(n * 2^n). **Space:** O(n) recursion.
3. Each node of recursion tree is a valid subset.
4. Duplicates need sorting + skip (LC 90).
5. Foundation for combinations, permutations.

---

### Q2. Permutations (LC 46)

**Problem:** Generate all permutations of distinct integers.

**C++ Solution (Backtracking with Used Array):**
```cpp
void backtrack(vector<int>& nums, vector<bool>& used, vector<int>& path, vector<vector<int>>& res) {
    if (path.size() == nums.size()) { res.push_back(path); return; }
    for (int i = 0; i < nums.size(); i++) {
        if (used[i]) continue;
        used[i] = true; path.push_back(nums[i]);
        backtrack(nums, used, path, res);
        path.pop_back(); used[i] = false;
    }
}
```

**Example:** `[1,2,3]` → 6 permutations.

**Key Points:**
1. **Pattern:** Backtracking with visited array.
2. **Time:** O(n * n!). **Space:** O(n).
3. Duplicates require sorting + skip condition.
4. Swap-based method avoids extra array.
5. Core for permutation and arrangement problems.

---

### Q3. Combination Sum (LC 39)

**Problem:** Find all combinations summing to target (unlimited use).

**C++ Solution:**
```cpp
void backtrack(vector<int>& cand, int target, int start, vector<int>& path, vector<vector<int>>& res) {
    if (target == 0) { res.push_back(path); return; }
    for (int i = start; i < cand.size(); i++) {
        if (cand[i] > target) break;
        path.push_back(cand[i]);
        backtrack(cand, target - cand[i], i, path, res);
        path.pop_back();
    }
}
```

**Example:** `candidates=[2,3,6,7], target=7` → `[[2,2,3],[7]]`.

**Key Points:**
1. **Pattern:** Backtracking with reuse (pass `i`, not `i+1`).
2. **Time:** Exponential. **Space:** O(target/min).
3. Sort to enable pruning.
4. Each number can be used unlimited times.
5. Variant with unique use (LC 40) passes `i+1`.

---

### Q4. Word Search (LC 79)

**Problem:** Find if a word exists in a 2D board (adjacent cells).

**C++ Solution (DFS Backtracking):**
```cpp
bool dfs(vector<vector<char>>& b, string& w, int i, int j, int k) {
    if (k == w.size()) return true;
    if (i<0||j<0||i>=b.size()||j>=b[0].size()||b[i][j]!=w[k]) return false;
    char tmp = b[i][j]; b[i][j] = '#';
    bool found = dfs(b,w,i+1,j,k+1) || dfs(b,w,i-1,j,k+1)
              || dfs(b,w,i,j+1,k+1) || dfs(b,w,i,j-1,k+1);
    b[i][j] = tmp;
    return found;
}
bool exist(vector<vector<char>>& board, string word) {
    for (int i=0;i<board.size();i++)
        for (int j=0;j<board[0].size();j++)
            if (dfs(board, word, i, j, 0)) return true;
    return false;
}
```

**Example:** Board with `"ABCCED"` → `true`.

**Key Points:**
1. **Pattern:** DFS + backtracking (mark visited).
2. **Time:** O(mn * 4^L). **Space:** O(L) recursion.
3. Restore cell after exploration.
4. Prune if first char mismatch.
5. Used in Boggle, crossword puzzles.

---

### Q5. N-Queens (LC 51)

**Problem:** Place n queens on n×n board with no two attacking.

**C++ Solution (Backtracking with Sets):**
```cpp
void solve(int n, int row, vector<int>& cols, unordered_set<int>& diag, unordered_set<int>& anti, vector<vector<string>>& res) {
    if (row == n) {
        vector<string> board(n, string(n, '.'));
        for (int r = 0; r < n; r++) board[r][cols[r]] = 'Q';
        res.push_back(board); return;
    }
    for (int c = 0; c < n; c++) {
        if (diag.count(row-c) || anti.count(row+c)) continue;
        cols[row] = c; diag.insert(row-c); anti.insert(row+c);
        solve(n, row+1, cols, diag, anti, res);
        diag.erase(row-c); anti.erase(row+c);
    }
}
```

**Example:** n=4 → 2 solutions.

**Key Points:**
1. **Pattern:** Backtracking with constraint sets.
2. **Time:** O(n!). **Space:** O(n).
3. `row-c` and `row+c` identify diagonals.
4. Place row by row, avoid columns.
5. Classic constraint satisfaction.

---

### Q6. Letter Combinations of a Phone Number (LC 17)

**Problem:** Generate all letter combinations from digits 2–9.

**C++ Solution:**
```cpp
void backtrack(string& digits, int idx, string& path, vector<string>& res, vector<string>& mp) {
    if (idx == digits.size()) { res.push_back(path); return; }
    for (char c : mp[digits[idx]-'0']) {
        path.push_back(c);
        backtrack(digits, idx+1, path, res, mp);
        path.pop_back();
    }
}
vector<string> letterCombinations(string digits) {
    if (digits.empty()) return {};
    vector<string> mp = {"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
    vector<string> res; string path;
    backtrack(digits, 0, path, res, mp);
    return res;
}
```

**Example:** `"23"` → `["ad","ae","af","bd","be","bf","cd","ce","cf"]`.

**Key Points:**
1. **Pattern:** Backtracking over digit choices.
2. **Time:** O(4^n * n). **Space:** O(n).
3. Mapping array for digits.
4. Handles empty input.
5. Foundation for phone keypad problems.

---

### Q7. Palindrome Partitioning (LC 131)

**Problem:** Partition a string into all palindrome substrings.

**C++ Solution (Backtracking + Palindrome Check):**
```cpp
bool isPal(string& s, int l, int r) {
    while (l < r) if (s[l++] != s[r--]) return false;
    return true;
}
void backtrack(string& s, int start, vector<string>& path, vector<vector<string>>& res) {
    if (start == s.size()) { res.push_back(path); return; }
    for (int end = start; end < s.size(); end++) {
        if (isPal(s, start, end)) {
            path.push_back(s.substr(start, end-start+1));
            backtrack(s, end+1, path, res);
            path.pop_back();
        }
    }
}
```

**Example:** `"aab"` → `[["a","a","b"],["aa","b"]]`.

**Key Points:**
1. **Pattern:** Backtracking with palindrome pruning.
2. **Time:** O(n * 2^n). **Space:** O(n).
3. Precompute palindrome table for O(1) checks.
4. Each partition point is a choice.
5. Similar to word break.

---

### Q8. Generate Parentheses (LC 22)

**Problem:** Generate all valid parentheses combinations of n pairs.

**C++ Solution:**
```cpp
void backtrack(int open, int close, int n, string& path, vector<string>& res) {
    if (path.size() == 2*n) { res.push_back(path); return; }
    if (open < n) { path.push_back('('); backtrack(open+1, close, n, path, res); path.pop_back(); }
    if (close < open) { path.push_back(')'); backtrack(open, close+1, n, path, res); path.pop_back(); }
}
```

**Example:** n=3 → 5 valid combinations.

**Key Points:**
1. **Pattern:** Backtracking with constraints.
2. **Time:** O(4^n / √n). **Space:** O(n).
3. Open count ≤ n; close count ≤ open.
4. Only add valid parentheses.
5. Catalan number count.

---

### Q9. Sudoku Solver (LC 37)

**Problem:** Solve a 9×9 Sudoku puzzle.

**C++ Solution (Backtracking with Sets):**
```cpp
bool solve(vector<vector<char>>& board, vector<unordered_set<char>>& rows,
           vector<unordered_set<char>>& cols, vector<unordered_set<char>>& boxes) {
    for (int i = 0; i < 9; i++)
        for (int j = 0; j < 9; j++)
            if (board[i][j] == '.') {
                int b = (i/3)*3 + j/3;
                for (char c = '1'; c <= '9'; c++) {
                    if (rows[i].count(c) || cols[j].count(c) || boxes[b].count(c)) continue;
                    board[i][j] = c; rows[i].insert(c); cols[j].insert(c); boxes[b].insert(c);
                    if (solve(board, rows, cols, boxes)) return true;
                    board[i][j] = '.'; rows[i].erase(c); cols[j].erase(c); boxes[b].erase(c);
                }
                return false;
            }
    return true;
}
```

**Example:** Solves any valid Sudoku puzzle.

**Key Points:**
1. **Pattern:** Backtracking with constraint propagation.
2. **Time:** Exponential worst case. **Space:** O(1) (9×9).
3. Sets track used digits per row/col/box.
4. Early return on first solution.
5. Hard but classic backtracking problem.

---

### Q10. Combination Sum II (LC 40)

**Problem:** Combinations summing to target, each number used once.

**C++ Solution (Sort + Skip Duplicates):**
```cpp
void backtrack(vector<int>& cand, int target, int start, vector<int>& path, vector<vector<int>>& res) {
    if (target == 0) { res.push_back(path); return; }
    for (int i = start; i < cand.size(); i++) {
        if (i > start && cand[i] == cand[i-1]) continue;
        if (cand[i] > target) break;
        path.push_back(cand[i]);
        backtrack(cand, target - cand[i], i+1, path, res);
        path.pop_back();
    }
}
```

**Example:** `candidates=[10,1,2,7,6,1,5], target=8` → `[[1,1,6],[1,2,5],[1,7],[2,6]]`.

**Key Points:**
1. **Pattern:** Backtracking with duplicate skipping.
2. **Time:** Exponential. **Space:** O(n).
3. Sort to bring duplicates together.
4. Skip if `i > start && cand[i] == cand[i-1]`.
5. Each number used at most once.

---

## 6. Prefix Sum and HashSets

### Q1. Two Sum (LC 1)

**Problem:** Find two numbers that add to target.

**C++ Solution (Hash Map):**
```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int,int> mp;
    for (int i = 0; i < nums.size(); i++) {
        if (mp.count(target - nums[i])) return {mp[target - nums[i]], i};
        mp[nums[i]] = i;
    }
    return {};
}
```

**Example:** `[2,7,11,15], target=9` → `[0,1]`.

**Key Points:**
1. **Pattern:** Hash map for complement lookup.
2. **Time:** O(n). **Space:** O(n).
3. One-pass: check complement before inserting.
4. Handles duplicates.
5. Foundation for 3Sum, 4Sum.

---

### Q2. Subarray Sum Equals K (LC 560)

**Problem:** Count subarrays with sum equal to k.

**C++ Solution (Prefix Sum + Hash Map):**
```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int,int> mp; mp[0] = 1;
    int sum = 0, count = 0;
    for (int x : nums) {
        sum += x;
        count += mp[sum - k];
        mp[sum]++;
    }
    return count;
}
```

**Example:** `[1,1,1], k=2` → 2.

**Key Points:**
1. **Pattern:** Prefix sum + frequency map.
2. **Time:** O(n). **Space:** O(n).
3. `mp[0]=1` handles subarrays starting at index 0.
4. `sum - k` is the needed prefix.
5. Core for subarray sum problems.

---

### Q3. Contiguous Array (LC 525)

**Problem:** Longest subarray with equal 0s and 1s.

**C++ Solution (Prefix Sum with 0→-1):**
```cpp
int findMaxLength(vector<int>& nums) {
    unordered_map<int,int> mp; mp[0] = -1;
    int sum = 0, maxLen = 0;
    for (int i = 0; i < nums.size(); i++) {
        sum += nums[i] ? 1 : -1;
        if (mp.count(sum)) maxLen = max(maxLen, i - mp[sum]);
        else mp[sum] = i;
    }
    return maxLen;
}
```

**Example:** `[0,1,0]` → 2.

**Key Points:**
1. **Pattern:** Prefix sum with value mapping.
2. **Time:** O(n). **Space:** O(n).
3. Map first occurrence of each sum.
4. Sum repeats → equal 0s and 1s between.
5. Similar to longest subarray with sum k.

---

### Q4. Product of Array Except Self (LC 238)

**Problem:** Output array where each element is product of all others (no division).

**C++ Solution (Prefix & Suffix Products):**
```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size(); vector<int> res(n, 1);
    for (int i = 1; i < n; i++) res[i] = res[i-1] * nums[i-1];
    int suffix = 1;
    for (int i = n-1; i >= 0; i--) {
        res[i] *= suffix;
        suffix *= nums[i];
    }
    return res;
}
```

**Example:** `[1,2,3,4]` → `[24,12,8,6]`.

**Key Points:**
1. **Pattern:** Prefix product + suffix product.
2. **Time:** O(n). **Space:** O(1) extra.
3. Two passes: left to right, then right to left.
4. No division needed.
5. Handles zeros correctly.

---

### Q5. Longest Consecutive Sequence (LC 128)

**Problem:** Longest consecutive sequence in unsorted array.

**C++ Solution (Hash Set):**
```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> s(nums.begin(), nums.end());
    int best = 0;
    for (int x : s) {
        if (s.count(x-1)) continue;
        int y = x;
        while (s.count(y)) y++;
        best = max(best, y - x);
    }
    return best;
}
```

**Example:** `[100,4,200,1,3,2]` → 4 (1,2,3,4).

**Key Points:**
1. **Pattern:** Hash set for O(1) lookups.
2. **Time:** O(n). **Space:** O(n).
3. Only start from sequence beginnings (`x-1` not present).
4. Each element visited at most twice.
5. Avoids sorting.

---

### Q6. Find All Numbers Disappeared in an Array (LC 448)

**Problem:** Find missing numbers from 1..n.

**C++ Solution (In-place Marking):**
```cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {
    for (int x : nums) {
        int idx = abs(x) - 1;
        if (nums[idx] > 0) nums[idx] = -nums[idx];
    }
    vector<int> res;
    for (int i = 0; i < nums.size(); i++)
        if (nums[i] > 0) res.push_back(i+1);
    return res;
}
```

**Example:** `[4,3,2,7,8,2,3,1]` → `[5,6]`.

**Key Points:**
1. **Pattern:** Index marking (no extra space).
2. **Time:** O(n). **Space:** O(1).
3. Mark present by negating value at index.
4. Positive value → missing.
5. Similar to find duplicates.

---

### Q7. Group Anagrams (LC 49)

**Problem:** Group strings that are anagrams.

**C++ Solution (Frequency Key):**
```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for (string& s : strs) {
        vector<int> cnt(26,0);
        for (char c : s) cnt[c-'a']++;
        string key;
        for (int c : cnt) key += to_string(c) + "#";
        mp[key].push_back(s);
    }
    vector<vector<string>> res;
    for (auto& [k,v] : mp) res.push_back(v);
    return res;
}
```

**Example:** `["eat","tea","tan","ate","nat","bat"]` → `[["bat"],["nat","tan"],["ate","eat","tea"]]`.

**Key Points:**
1. **Pattern:** Hash map with normalized key.
2. **Time:** O(n * L). **Space:** O(nL).
3. Sorting key also works (O(L log L)).
4. Frequency key avoids sorting.
5. Classic hashing problem.

---

### Q8. Top K Frequent Elements (LC 347)

**Problem:** Return k most frequent elements.

**C++ Solution (Bucket Sort):**
```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int,int> freq;
    for (int x : nums) freq[x]++;
    vector<vector<int>> buckets(nums.size()+1);
    for (auto& [num, f] : freq) buckets[f].push_back(num);
    vector<int> res;
    for (int i = buckets.size()-1; i >= 0 && res.size() < k; i--)
        for (int num : buckets[i]) {
            res.push_back(num);
            if (res.size() == k) break;
        }
    return res;
}
```

**Example:** `[1,1,1,2,2,3], k=2` → `[1,2]`.

**Key Points:**
1. **Pattern:** Frequency map + bucket sort.
2. **Time:** O(n). **Space:** O(n).
3. Bucket index = frequency.
4. Avoids O(n log n) heap sort.
5. Min-heap O(n log k) also works.

---

### Q9. Valid Sudoku (LC 36)

**Problem:** Validate a partially filled Sudoku board.

**C++ Solution (Hash Sets):**
```cpp
bool isValidSudoku(vector<vector<char>>& b) {
    vector<unordered_set<char>> rows(9), cols(9), boxes(9);
    for (int i = 0; i < 9; i++)
        for (int j = 0; j < 9; j++) {
            char c = b[i][j];
            if (c == '.') continue;
            int box = (i/3)*3 + j/3;
            if (rows[i].count(c) || cols[j].count(c) || boxes[box].count(c)) return false;
            rows[i].insert(c); cols[j].insert(c); boxes[box].insert(c);
        }
    return true;
}
```

**Example:** Valid board → `true`.

**Key Points:**
1. **Pattern:** Hash sets for constraints.
2. **Time:** O(1) (81 cells). **Space:** O(1).
3. Check row, column, 3×3 box.
4. Box index formula `(i/3)*3 + j/3`.
5. Foundation for Sudoku Solver.

---

### Q10. Longest Substring Without Repeating Characters (LC 3)

**Problem:** Longest substring without repeating characters.

**C++ Solution (Sliding Window + Hash Map):**
```cpp
int lengthOfLongestSubstring(string s) {
    unordered_map<char,int> mp;
    int left = 0, maxLen = 0;
    for (int right = 0; right < s.size(); right++) {
        if (mp.count(s[right])) left = max(left, mp[s[right]] + 1);
        mp[s[right]] = right;
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**Example:** `"abcabcbb"` → 3 (`"abc"`).

**Key Points:**
1. **Pattern:** Sliding window with char→index map.
2. **Time:** O(n). **Space:** O(min(n, 26)).
3. Move left past duplicate.
4. Map stores last index.
5. Core sliding window problem.

---

## 7. Sliding Window and HashMap

### Q1. Minimum Window Substring (LC 76)

**Problem:** Minimum window in s containing all chars of t.

**C++ Solution (Sliding Window + Counter):**
```cpp
string minWindow(string s, string t) {
    unordered_map<char,int> need, window;
    for (char c : t) need[c]++;
    int left = 0, right = 0, valid = 0, start = 0, len = INT_MAX;
    while (right < s.size()) {
        char c = s[right++];
        if (need.count(c)) {
            window[c]++;
            if (window[c] == need[c]) valid++;
        }
        while (valid == need.size()) {
            if (right - left < len) { start = left; len = right - left; }
            char d = s[left++];
            if (need.count(d)) {
                if (window[d] == need[d]) valid--;
                window[d]--;
            }
        }
    }
    return len == INT_MAX ? "" : s.substr(start, len);
}
```

**Example:** `s="ADOBECODEBANC", t="ABC"` → `"BANC"`.

**Key Points:**
1. **Pattern:** Variable-size sliding window.
2. **Time:** O(n). **Space:** O(k).
3. `valid` tracks matched characters.
4. Expand right, shrink left when valid.
5. Hardest sliding window problem.

---

### Q2. Longest Repeating Character Replacement (LC 424)

**Problem:** Longest substring with same char after at most k replacements.

**C++ Solution:**
```cpp
int characterReplacement(string s, int k) {
    vector<int> cnt(26,0);
    int left = 0, maxFreq = 0, maxLen = 0;
    for (int right = 0; right < s.size(); right++) {
        maxFreq = max(maxFreq, ++cnt[s[right]-'A']);
        while (right - left + 1 - maxFreq > k) cnt[s[left++]-'A']--;
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**Example:** `s="AABABBA", k=1` → 4.

**Key Points:**
1. **Pattern:** Sliding window with frequency.
2. **Time:** O(n). **Space:** O(1).
3. Window valid if `len - maxFreq ≤ k`.
4. `maxFreq` can only increase.
5. No need to shrink below max length.

---

### Q3. Permutation in String (LC 567)

**Problem:** Check if s2 contains a permutation of s1.

**C++ Solution (Fixed Window + Frequency):**
```cpp
bool checkInclusion(string s1, string s2) {
    if (s1.size() > s2.size()) return false;
    vector<int> need(26,0), window(26,0);
    for (char c : s1) need[c-'a']++;
    for (int i = 0; i < s1.size(); i++) window[s2[i]-'a']++;
    if (need == window) return true;
    for (int i = s1.size(); i < s2.size(); i++) {
        window[s2[i]-'a']++;
        window[s2[i-s1.size()]-'a']--;
        if (need == window) return true;
    }
    return false;
}
```

**Example:** `s1="ab", s2="eidbaooo"` → `true`.

**Key Points:**
1. **Pattern:** Fixed-size sliding window.
2. **Time:** O(n). **Space:** O(1).
3. Compare frequency arrays.
4. Window size = s1.size().
5. Similar to Find All Anagrams.

---

### Q4. Find All Anagrams in a String (LC 438)

**Problem:** Find all start indices of anagrams of p in s.

**C++ Solution (Fixed Window):**
```cpp
vector<int> findAnagrams(string s, string p) {
    vector<int> need(26,0), window(26,0), res;
    if (p.size() > s.size()) return res;
    for (char c : p) need[c-'a']++;
    for (int i = 0; i < s.size(); i++) {
        window[s[i]-'a']++;
        if (i >= p.size()) window[s[i-p.size()]-'a']--;
        if (need == window) res.push_back(i - p.size() + 1);
    }
    return res;
}
```

**Example:** `s="cbaebabacd", p="abc"` → `[0,6]`.

**Key Points:**
1. **Pattern:** Fixed window + frequency match.
2. **Time:** O(n). **Space:** O(1).
3. Window slides by adding and removing.
4. Compare arrays each step.
5. Classic anagram detection.

---

### Q5. Subarrays with K Different Integers (LC 992)

**Problem:** Count subarrays with exactly k distinct integers.

**C++ Solution (At Most K – At Most K-1):**
```cpp
int atMost(vector<int>& nums, int k) {
    unordered_map<int,int> mp;
    int left = 0, res = 0;
    for (int right = 0; right < nums.size(); right++) {
        if (mp[nums[right]]++ == 0) k--;
        while (k < 0) if (--mp[nums[left++]] == 0) k++;
        res += right - left + 1;
    }
    return res;
}
int subarraysWithKDistinct(vector<int>& nums, int k) {
    return atMost(nums, k) - atMost(nums, k-1);
}
```

**Example:** `[1,2,1,2,3], k=2` → 7.

**Key Points:**
1. **Pattern:** Exactly K = AtMost(K) − AtMost(K-1).
2. **Time:** O(n). **Space:** O(k).
3. At most K counts subarrays with ≤ K distinct.
4. Shrink when distinct > k.
5. Elegant counting technique.

---

### Q6. Fruit Into Baskets (LC 904)

**Problem:** Longest subarray with at most 2 distinct values.

**C++ Solution:**
```cpp
int totalFruit(vector<int>& fruits) {
    unordered_map<int,int> mp;
    int left = 0, maxLen = 0;
    for (int right = 0; right < fruits.size(); right++) {
        mp[fruits[right]]++;
        while (mp.size() > 2) {
            if (--mp[fruits[left]] == 0) mp.erase(fruits[left]);
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**Example:** `[1,2,1]` → 3; `[0,1,2,2]` → 3.

**Key Points:**
1. **Pattern:** Sliding window with ≤ 2 distinct.
2. **Time:** O(n). **Space:** O(1).
3. Map counts fruit types in window.
4. Shrink when > 2 types.
5. Same as at most 2 distinct.

---

### Q7. Maximum Points You Can Obtain from Cards (LC 1423)

**Problem:** Max points taking k cards from either end.

**C++ Solution (Sliding Window on Complement):**
```cpp
int maxScore(vector<int>& card, int k) {
    int n = card.size(), total = accumulate(card.begin(), card.end(), 0);
    int window = n - k, sum = 0, minSum = INT_MAX;
    if (window == 0) return total;
    for (int i = 0; i < n; i++) {
        sum += card[i];
        if (i >= window) sum -= card[i-window];
        if (i >= window-1) minSum = min(minSum, sum);
    }
    return total - minSum;
}
```

**Example:** `[1,2,3,4,5,6,1], k=3` → 12.

**Key Points:**
1. **Pattern:** Total minus minimum middle window.
2. **Time:** O(n). **Space:** O(1).
3. Middle window size = n − k.
4. Equivalent to removing n−k consecutive cards.
5. Clever transformation.

---

### Q8. Minimum Size Subarray Sum (LC 209)

**Problem:** Minimal length subarray with sum ≥ target.

**C++ Solution:**
```cpp
int minSubArrayLen(int target, vector<int>& nums) {
    int left = 0, sum = 0, minLen = INT_MAX;
    for (int right = 0; right < nums.size(); right++) {
        sum += nums[right];
        while (sum >= target) {
            minLen = min(minLen, right - left + 1);
            sum -= nums[left++];
        }
    }
    return minLen == INT_MAX ? 0 : minLen;
}
```

**Example:** `target=7, nums=[2,3,1,2,4,3]` → 2 (`[4,3]`).

**Key Points:**
1. **Pattern:** Variable window sum.
2. **Time:** O(n). **Space:** O(1).
3. Expand right, shrink left while sum ≥ target.
4. Positive numbers required.
5. Foundation for subarray sum problems.

---

### Q9. Longest Substring with At Most K Distinct Characters (LC 340)

**Problem:** Longest substring with ≤ k distinct characters.

**C++ Solution:**
```cpp
int lengthOfLongestSubstringKDistinct(string s, int k) {
    unordered_map<char,int> mp;
    int left = 0, maxLen = 0;
    for (int right = 0; right < s.size(); right++) {
        mp[s[right]]++;
        while (mp.size() > k) {
            if (--mp[s[left]] == 0) mp.erase(s[left]);
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**Example:** `s="eceba", k=2` → 3 (`"ece"`).

**Key Points:**
1. **Pattern:** Sliding window with distinct constraint.
2. **Time:** O(n). **Space:** O(k).
3. Map counts chars in window.
4. Shrink when distinct > k.
5. Generalizes Fruit Into Baskets.

---

### Q10. Sliding Window Median (LC 480)

**Problem:** Median of each sliding window of size k.

**C++ Solution (Multiset / Two Heaps):**
```cpp
vector<double> medianSlidingWindow(vector<int>& nums, int k) {
    multiset<int> window(nums.begin(), nums.begin()+k);
    auto mid = next(window.begin(), k/2);
    vector<double> res;
    for (int i = k; ; i++) {
        res.push_back((double)(*mid + *prev(mid, 1-k%2)) / 2);
        if (i == nums.size()) break;
        window.insert(nums[i]);
        if (nums[i] < *mid) mid--;
        if (nums[i-k] <= *mid) mid++;
        window.erase(window.find(nums[i-k]));
    }
    return res;
}
```

**Example:** `[1,3,-1,-3,5,3,6,7], k=3` → `[1,-1,-1,3,5,6]`.

**Key Points:**
1. **Pattern:** Sliding window + balanced BST.
2. **Time:** O(n log k). **Space:** O(k).
3. Multiset maintains sorted order.
4. `mid` iterator tracks median.
5. Hard but classic.

---

## 8. Searching and Sorting

### Q1. Binary Search (LC 704)

**Problem:** Search target in sorted array.

**C++ Solution:**
```cpp
int search(vector<int>& nums, int target) {
    int lo = 0, hi = nums.size()-1;
    while (lo <= hi) {
        int mid = lo + (hi-lo)/2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) lo = mid+1;
        else hi = mid-1;
    }
    return -1;
}
```

**Example:** `[-1,0,3,5,9,12], target=9` → 4.

**Key Points:**
1. **Pattern:** Classic binary search.
2. **Time:** O(log n). **Space:** O(1).
3. `lo + (hi-lo)/2` avoids overflow.
4. Sorted array required.
5. Foundation for all binary search variants.

---

### Q2. Search in Rotated Sorted Array (LC 33)

**Problem:** Search in rotated sorted array.

**C++ Solution:**
```cpp
int search(vector<int>& nums, int target) {
    int lo = 0, hi = nums.size()-1;
    while (lo <= hi) {
        int mid = lo + (hi-lo)/2;
        if (nums[mid] == target) return mid;
        if (nums[lo] <= nums[mid]) {
            if (target >= nums[lo] && target < nums[mid]) hi = mid-1;
            else lo = mid+1;
        } else {
            if (target > nums[mid] && target <= nums[hi]) lo = mid+1;
            else hi = mid-1;
        }
    }
    return -1;
}
```

**Example:** `[4,5,6,7,0,1,2], target=0` → 4.

**Key Points:**
1. **Pattern:** Binary search with rotation check.
2. **Time:** O(log n). **Space:** O(1).
3. One half is always sorted.
4. Determine sorted half, then check range.
5. Handle duplicates by shrinking (LC 81).

---

### Q3. Find Minimum in Rotated Sorted Array (LC 153)

**Problem:** Find minimum in rotated sorted array.

**C++ Solution:**
```cpp
int findMin(vector<int>& nums) {
    int lo = 0, hi = nums.size()-1;
    while (lo < hi) {
        int mid = lo + (hi-lo)/2;
        if (nums[mid] > nums[hi]) lo = mid+1;
        else hi = mid;
    }
    return nums[lo];
}
```

**Example:** `[3,4,5,1,2]` → 1.

**Key Points:**
1. **Pattern:** Binary search on rotation point.
2. **Time:** O(log n). **Space:** O(1).
3. Compare mid with hi.
4. If mid > hi, min is right; else left.
5. Handle duplicates with `hi--`.

---

### Q4. Koko Eating Bananas (LC 875)

**Problem:** Minimum speed to eat all bananas in h hours.

**C++ Solution (Binary Search on Answer):**
```cpp
int minEatingSpeed(vector<int>& piles, int h) {
    int lo = 1, hi = *max_element(piles.begin(), piles.end());
    while (lo < hi) {
        int mid = lo + (hi-lo)/2;
        int hours = 0;
        for (int p : piles) hours += (p + mid - 1) / mid;
        if (hours <= h) hi = mid;
        else lo = mid+1;
    }
    return lo;
}
```

**Example:** `piles=[3,6,7,11], h=8` → 4.

**Key Points:**
1. **Pattern:** Binary search on answer space.
2. **Time:** O(n log max). **Space:** O(1).
3. Feasibility check `hours ≤ h`.
4. Find minimum feasible speed.
5. Classic “minimize maximum” problem.

---

### Q5. Merge Intervals (LC 56)

**Problem:** Merge overlapping intervals.

**C++ Solution (Sort + Merge):**
```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> res;
    for (auto& in : intervals) {
        if (res.empty() || res.back()[1] < in[0]) res.push_back(in);
        else res.back()[1] = max(res.back()[1], in[1]);
    }
    return res;
}
```

**Example:** `[[1,3],[2,6],[8,10],[15,18]]` → `[[1,6],[8,10],[15,18]]`.

**Key Points:**
1. **Pattern:** Sort by start, merge overlapping.
2. **Time:** O(n log n). **Space:** O(n).
3. Overlap if current start ≤ previous end.
4. Extend end to max.
5. Foundation for interval scheduling.

---

### Q6. Sort Colors (Dutch National Flag) (LC 75)

**Problem:** Sort array of 0s, 1s, 2s in-place.

**C++ Solution:**
```cpp
void sortColors(vector<int>& nums) {
    int lo = 0, mid = 0, hi = nums.size()-1;
    while (mid <= hi) {
        if (nums[mid] == 0) swap(nums[lo++], nums[mid++]);
        else if (nums[mid] == 1) mid++;
        else swap(nums[mid], nums[hi--]);
    }
}
```

**Example:** `[2,0,2,1,1,0]` → `[0,0,1,1,2,2]`.

**Key Points:**
1. **Pattern:** Three-pointer partition (Dutch flag).
2. **Time:** O(n). **Space:** O(1).
3. `lo` boundary for 0s, `hi` for 2s.
4. `mid` scans.
5. Used in quicksort partition.

---

### Q7. Kth Largest Element in an Array (LC 215)

**Problem:** Find kth largest element.

**C++ Solution (Quickselect):**
```cpp
int findKthLargest(vector<int>& nums, int k) {
    nth_element(nums.begin(), nums.begin()+k-1, nums.end(), greater<int>());
    return nums[k-1];
}
```

**Example:** `[3,2,1,5,6,4], k=2` → 5.

**Key Points:**
1. **Pattern:** Quickselect / heap.
2. **Time:** O(n) average, O(n²) worst. **Space:** O(1).
3. `nth_element` uses introselect.
4. Min-heap O(n log k) also works.
5. Avoid full sort O(n log n).

---

### Q8. Search a 2D Matrix (LC 74)

**Problem:** Search in row-column sorted matrix.

**C++ Solution (Binary Search as 1D):**
```cpp
bool searchMatrix(vector<vector<int>>& mat, int target) {
    int m = mat.size(), n = mat[0].size();
    int lo = 0, hi = m*n-1;
    while (lo <= hi) {
        int mid = lo + (hi-lo)/2;
        int val = mat[mid/n][mid%n];
        if (val == target) return true;
        else if (val < target) lo = mid+1;
        else hi = mid-1;
    }
    return false;
}
```

**Example:** 3×4 matrix → search 3.

**Key Points:**
1. **Pattern:** Binary search on virtual 1D array.
2. **Time:** O(log(mn)). **Space:** O(1).
3. Index mapping `mid/n`, `mid%n`.
4. Requires full sorted order.
5. Variant LC 240 uses staircase search.

---

### Q9. Median of Two Sorted Arrays (LC 4)

**Problem:** Find median of two sorted arrays.

**C++ Solution (Binary Search on Partition):**
```cpp
double findMedianSortedArrays(vector<int>& A, vector<int>& B) {
    if (A.size() > B.size()) swap(A, B);
    int m = A.size(), n = B.size(), lo = 0, hi = m;
    while (lo <= hi) {
        int i = (lo+hi)/2, j = (m+n+1)/2 - i;
        int maxL = max(i?A[i-1]:INT_MIN, j?B[j-1]:INT_MIN);
        int minR = min(i<m?A[i]:INT_MAX, j<n?B[j]:INT_MAX);
        if (maxL <= minR) {
            if ((m+n)%2) return maxL;
            return (maxL + minR) / 2.0;
        } else if (i > 0 && A[i-1] > B[j]) hi = i-1;
        else lo = i+1;
    }
    return 0;
}
```

**Example:** `[1,3]` and `[2]` → 2.0.

**Key Points:**
1. **Pattern:** Binary search on partition of smaller array.
2. **Time:** O(log min(m,n)). **Space:** O(1).
3. Ensure left half ≤ right half.
4. Handle even/odd total length.
5. Hard but optimal.

---

### Q10. First Bad Version (LC 278)

**Problem:** Find first bad version (API `isBadVersion`).

**C++ Solution:**
```cpp
int firstBadVersion(int n) {
    int lo = 1, hi = n;
    while (lo < hi) {
        int mid = lo + (hi-lo)/2;
        if (isBadVersion(mid)) hi = mid;
        else lo = mid+1;
    }
    return lo;
}
```

**Example:** n=5, first bad=4 → 4.

**Key Points:**
1. **Pattern:** Binary search for boundary.
2. **Time:** O(log n). **Space:** O(1).
3. `hi = mid` when bad; `lo = mid+1` when good.
4. Avoid overflow with `lo + (hi-lo)/2`.
5. Classic “first true” pattern.

---

## 9. Two Pointers

### Q1. Valid Palindrome (LC 125)

**Problem:** Check if string is palindrome (alphanumeric only).

**C++ Solution:**
```cpp
bool isPalindrome(string s) {
    int l = 0, r = s.size()-1;
    while (l < r) {
        while (l < r && !isalnum(s[l])) l++;
        while (l < r && !isalnum(s[r])) r--;
        if (tolower(s[l]) != tolower(s[r])) return false;
        l++; r--;
    }
    return true;
}
```

**Example:** `"A man, a plan, a canal: Panama"` → `true`.

**Key Points:**
1. **Pattern:** Two pointers inward.
2. **Time:** O(n). **Space:** O(1).
3. Skip non-alphanumeric.
4. Case-insensitive.
5. Foundation for palindrome problems.

---

### Q2. Two Sum II (LC 167)

**Problem:** Two numbers in sorted array sum to target.

**C++ Solution:**
```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    int l = 0, r = nums.size()-1;
    while (l < r) {
        int sum = nums[l] + nums[r];
        if (sum == target) return {l+1, r+1};
        else if (sum < target) l++;
        else r--;
    }
    return {};
}
```

**Example:** `[2,7,11,15], target=9` → `[1,2]`.

**Key Points:**
1. **Pattern:** Two pointers on sorted array.
2. **Time:** O(n). **Space:** O(1).
3. Move left to increase sum, right to decrease.
4. Sorted input required.
5. O(n²) brute force improved.

---

### Q3. 3Sum (LC 15)

**Problem:** Find all triplets summing to zero.

**C++ Solution (Sort + Two Pointers):**
```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    for (int i = 0; i < nums.size(); i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        int l = i+1, r = nums.size()-1;
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum == 0) {
                res.push_back({nums[i], nums[l], nums[r]});
                while (l < r && nums[l] == nums[l+1]) l++;
                while (l < r && nums[r] == nums[r-1]) r--;
                l++; r--;
            } else if (sum < 0) l++;
            else r--;
        }
    }
    return res;
}
```

**Example:** `[-1,0,1,2,-1,-4]` → `[[-1,-1,2],[-1,0,1]]`.

**Key Points:**
1. **Pattern:** Sort + fix one + two pointers.
2. **Time:** O(n²). **Space:** O(1) extra.
3. Skip duplicates for i, l, r.
4. Avoids O(n³) brute force.
5. Extends to 4Sum.

---

### Q4. Container With Most Water (LC 11)

**Problem:** Max water between two vertical lines.

**C++ Solution:**
```cpp
int maxArea(vector<int>& height) {
    int l = 0, r = height.size()-1, maxA = 0;
    while (l < r) {
        maxA = max(maxA, min(height[l], height[r]) * (r-l));
        if (height[l] < height[r]) l++;
        else r--;
    }
    return maxA;
}
```

**Example:** `[1,8,6,2,5,4,8,3,7]` → 49.

**Key Points:**
1. **Pattern:** Two pointers moving shorter side.
2. **Time:** O(n). **Space:** O(1).
3. Area limited by shorter line.
4. Move shorter pointer inward.
5. Greedy proof: can’t improve by moving taller.

---

### Q5. Trapping Rain Water (LC 42)

**Problem:** Trap rainwater between bars.

**C++ Solution (Two Pointers):**
```cpp
int trap(vector<int>& height) {
    int l = 0, r = height.size()-1, leftMax = 0, rightMax = 0, water = 0;
    while (l < r) {
        if (height[l] < height[r]) {
            leftMax = max(leftMax, height[l]);
            water += leftMax - height[l++];
        } else {
            rightMax = max(rightMax, height[r]);
            water += rightMax - height[r--];
        }
    }
    return water;
}
```

**Example:** `[0,1,0,2,1,0,1,3,2,1,2,1]` → 6.

**Key Points:**
1. **Pattern:** Two pointers + running max.
2. **Time:** O(n). **Space:** O(1).
3. Water at i = min(leftMax, rightMax) − height[i].
4. Process smaller side first.
5. Stack solution also works.

---

### Q6. Remove Duplicates from Sorted Array (LC 26)

**Problem:** Remove duplicates in-place, return new length.

**C++ Solution (Slow/Fast Pointers):**
```cpp
int removeDuplicates(vector<int>& nums) {
    if (nums.empty()) return 0;
    int slow = 0;
    for (int fast = 1; fast < nums.size(); fast++)
        if (nums[fast] != nums[slow]) nums[++slow] = nums[fast];
    return slow + 1;
}
```

**Example:** `[1,1,2]` → length 2, `[1,2,_]`.

**Key Points:**
1. **Pattern:** Slow/fast pointers.
2. **Time:** O(n). **Space:** O(1).
3. `slow` writes next unique.
4. In-place modification.
5. Similar for remove element, move zeros.

---

### Q7. Squares of a Sorted Array (LC 977)

**Problem:** Squares of sorted array, sorted.

**C++ Solution (Two Pointers from Ends):**
```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int n = nums.size(); vector<int> res(n);
    int l = 0, r = n-1, idx = n-1;
    while (l <= r) {
        int a = nums[l]*nums[l], b = nums[r]*nums[r];
        if (a > b) { res[idx--] = a; l++; }
        else { res[idx--] = b; r--; }
    }
    return res;
}
```

**Example:** `[-4,-1,0,3,10]` → `[0,1,9,16,100]`.

**Key Points:**
1. **Pattern:** Two pointers from largest square.
2. **Time:** O(n). **Space:** O(n).
3. Largest squares at ends.
4. Fill result from end.
5. Avoids O(n log n) sort.

---

### Q8. 4Sum (LC 18)

**Problem:** Find unique quadruplets summing to target.

**C++ Solution (Sort + Nested Two Pointers):**
```cpp
vector<vector<int>> fourSum(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    int n = nums.size();
    for (int i = 0; i < n; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        for (int j = i+1; j < n; j++) {
            if (j > i+1 && nums[j] == nums[j-1]) continue;
            int l = j+1, r = n-1;
            while (l < r) {
                long long sum = (long long)nums[i]+nums[j]+nums[l]+nums[r];
                if (sum == target) {
                    res.push_back({nums[i], nums[j], nums[l], nums[r]});
                    while (l < r && nums[l] == nums[l+1]) l++;
                    while (l < r && nums[r] == nums[r-1]) r--;
                    l++; r--;
                } else if (sum < target) l++;
                else r--;
            }
        }
    }
    return res;
}
```

**Example:** `[1,0,-1,0,-2,2], target=0` → 3 quadruplets.

**Key Points:**
1. **Pattern:** Sort + two loops + two pointers.
2. **Time:** O(n³). **Space:** O(1) extra.
3. Use `long long` for sums.
4. Skip duplicates at all levels.
5. Generalizes to kSum.

---

### Q9. Sort Array By Parity (LC 905)

**Problem:** Move evens before odds.

**C++ Solution (Two Pointers):**
```cpp
vector<int> sortArrayByParity(vector<int>& nums) {
    int l = 0, r = nums.size()-1;
    while (l < r) {
        if (nums[l] % 2) swap(nums[l], nums[r--]);
        else l++;
    }
    return nums;
}
```

**Example:** `[3,1,2,4]` → `[2,4,3,1]`.

**Key Points:**
1. **Pattern:** Partition with two pointers.
2. **Time:** O(n). **Space:** O(1).
3. Left pointer seeks odd, right seeks even.
4. Swap when needed.
5. Similar to Dutch flag.

---

### Q10. Backspace String Compare (LC 844)

**Problem:** Compare two strings with `#` as backspace.

**C++ Solution (Two Pointers from End):**
```cpp
bool backspaceCompare(string s, string t) {
    int i = s.size()-1, j = t.size()-1;
    while (i >= 0 || j >= 0) {
        int skipS = 0, skipT = 0;
        while (i >= 0) {
            if (s[i] == '#') { skipS++; i--; }
            else if (skipS > 0) { skipS--; i--; }
            else break;
        }
        while (j >= 0) {
            if (t[j] == '#') { skipT++; j--; }
            else if (skipT > 0) { skipT--; j--; }
            else break;
        }
        if (i >= 0 && j >= 0 && s[i] != t[j]) return false;
        if ((i >= 0) != (j >= 0)) return false;
        i--; j--;
    }
    return true;
}
```

**Example:** `s="ab#c", t="ad#c"` → `true`.

**Key Points:**
1. **Pattern:** Two pointers from end with skip counts.
2. **Time:** O(n+m). **Space:** O(1).
3. Process backspaces without building strings.
4. Compare remaining chars.
5. Elegant O(1) space solution.

---

## 10. Greedy and Bit Manipulation

### Q1. Jump Game (LC 55)

**Problem:** Can you reach the last index?

**C++ Solution (Greedy):**
```cpp
bool canJump(vector<int>& nums) {
    int maxReach = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (i > maxReach) return false;
        maxReach = max(maxReach, i + nums[i]);
    }
    return true;
}
```

**Example:** `[2,3,1,1,4]` → `true`.

**Key Points:**
1. **Pattern:** Greedy max reach.
2. **Time:** O(n). **Space:** O(1).
3. Track farthest reachable index.
4. If current index > maxReach, fail.
5. Jump Game II counts min jumps.

---

### Q2. Merge Intervals (Greedy) (LC 56)

**Problem:** Merge overlapping intervals (greedy approach).

**C++ Solution (Sort by End or Start):**
```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> res;
    for (auto& in : intervals) {
        if (res.empty() || res.back()[1] < in[0]) res.push_back(in);
        else res.back()[1] = max(res.back()[1], in[1]);
    }
    return res;
}
```

**Example:** `[[1,3],[2,6],[8,10]]` → `[[1,6],[8,10]]`.

**Key Points:**
1. **Pattern:** Greedy after sorting.
2. **Time:** O(n log n). **Space:** O(n).
3. Extend current interval if overlapping.
4. Start new interval otherwise.
5. Foundation for interval scheduling.

---

### Q3. Non-overlapping Intervals (LC 435)

**Problem:** Min intervals to remove to make rest non-overlapping.

**C++ Solution (Sort by End):**
```cpp
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end(), [](auto& a, auto& b){ return a[1] < b[1]; });
    int count = 0, end = INT_MIN;
    for (auto& in : intervals) {
        if (in[0] >= end) end = in[1];
        else count++;
    }
    return count;
}
```

**Example:** `[[1,2],[2,3],[3,4],[1,3]]` → 1.

**Key Points:**
1. **Pattern:** Greedy interval scheduling.
2. **Time:** O(n log n). **Space:** O(1).
3. Choose interval with earliest end.
4. Remove overlapping ones.
5. Same as max non-overlapping.

---

### Q4. Gas Station (LC 134)

**Problem:** Find starting gas station for a circular tour.

**C++ Solution (Greedy):**
```cpp
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int total = 0, tank = 0, start = 0;
    for (int i = 0; i < gas.size(); i++) {
        total += gas[i] - cost[i];
        tank += gas[i] - cost[i];
        if (tank < 0) { start = i+1; tank = 0; }
    }
    return total >= 0 ? start : -1;
}
```

**Example:** `gas=[1,2,3,4,5], cost=[3,4,5,1,2]` → 3.

**Key Points:**
1. **Pattern:** Greedy reset at deficit.
2. **Time:** O(n). **Space:** O(1).
3. Total gas ≥ total cost necessary.
4. If tank negative, start after i.
5. Unique solution if total ≥ 0.

---

### Q5. Task Scheduler (LC 621)

**Problem:** Min intervals to schedule tasks with cooldown n.

**C++ Solution (Greedy Counting):**
```cpp
int leastInterval(vector<char>& tasks, int n) {
    vector<int> freq(26,0);
    for (char t : tasks) freq[t-'A']++;
    int maxF = *max_element(freq.begin(), freq.end());
    int maxCount = count(freq.begin(), freq.end(), maxF);
    return max((int)tasks.size(), (maxF-1)*(n+1) + maxCount);
}
```

**Example:** `tasks=["A","A","A","B","B","B"], n=2` → 8.

**Key Points:**
1. **Pattern:** Greedy based on max frequency.
2. **Time:** O(n). **Space:** O(1).
3. Formula: `(maxF-1)*(n+1) + maxCount`.
4. Take max with task count.
5. Idle slots determined by most frequent.

---

### Q6. Single Number (LC 136)

**Problem:** Find the element appearing once (others twice).

**C++ Solution (XOR):**
```cpp
int singleNumber(vector<int>& nums) {
    int res = 0;
    for (int x : nums) res ^= x;
    return res;
}
```

**Example:** `[4,1,2,1,2]` → 4.

**Key Points:**
1. **Pattern:** XOR cancels duplicates.
2. **Time:** O(n). **Space:** O(1).
3. `a ^ a = 0`, `a ^ 0 = a`.
4. XOR is commutative and associative.
5. Foundation for bit manipulation.

---

### Q7. Number of 1 Bits (LC 191)

**Problem:** Count set bits (Hamming weight).

**C++ Solution (Brian Kernighan):**
```cpp
int hammingWeight(uint32_t n) {
    int count = 0;
    while (n) { n &= (n-1); count++; }
    return count;
}
```

**Example:** `n=11 (1011)` → 3.

**Key Points:**
1. **Pattern:** `n & (n-1)` clears lowest set bit.
2. **Time:** O(number of set bits). **Space:** O(1).
3. Faster than checking all 32 bits.
4. Built-in `__builtin_popcount`.
5. Used in counting bits problems.

---

### Q8. Counting Bits (LC 338)

**Problem:** Count bits for all numbers 0..n.

**C++ Solution (DP):**
```cpp
vector<int> countBits(int n) {
    vector<int> dp(n+1);
    for (int i = 1; i <= n; i++)
        dp[i] = dp[i >> 1] + (i & 1);
    return dp;
}
```

**Example:** n=5 → `[0,1,1,2,1,2]`.

**Key Points:**
1. **Pattern:** DP with bit shift.
2. **Time:** O(n). **Space:** O(n).
3. `dp[i] = dp[i/2] + (i%2)`.
4. Or `dp[i] = dp[i & (i-1)] + 1`.
5. Combines DP and bit manipulation.

---

### Q9. Missing Number (LC 268)

**Problem:** Find missing number from 0..n.

**C++ Solution (XOR):**
```cpp
int missingNumber(vector<int>& nums) {
    int res = nums.size();
    for (int i = 0; i < nums.size(); i++)
        res ^= i ^ nums[i];
    return res;
}
```

**Example:** `[3,0,1]` → 2.

**Key Points:**
1. **Pattern:** XOR all indices and values.
2. **Time:** O(n). **Space:** O(1).
3. Duplicates cancel, missing remains.
4. Alternative: sum formula.
5. Handles missing n.

---

### Q10. Reverse Bits (LC 190)

**Problem:** Reverse bits of a 32-bit integer.

**C++ Solution:**
```cpp
uint32_t reverseBits(uint32_t n) {
    uint32_t res = 0;
    for (int i = 0; i < 32; i++) {
        res = (res << 1) | (n & 1);
        n >>= 1;
    }
    return res;
}
```

**Example:** `00000010100101000001111010011100` → `00111001011110000010100101000000`.

**Key Points:**
1. **Pattern:** Bit-by-bit reversal.
2. **Time:** O(32) = O(1). **Space:** O(1).
3. Shift result left, add LSB of n.
4. Shift n right.
5. Can optimize with bit tricks.

---

## Quick Revision Checklist

| Pattern | Must-Know Problems |
|---|---|
| Linked List | Reverse, Cycle, Merge, Remove Nth |
| Stack & Queue | Valid Parentheses, Min Stack, Monotonic Stack |
| Graphs & Trees | Islands, Topological Sort, Dijkstra, LCA |
| DP | Knapsack, LCS, LIS, Coin Change |
| Recursion & Backtracking | Subsets, Permutations, N-Queens |
| Prefix Sum & HashSets | Subarray Sum = K, Two Sum |
| Sliding Window & HashMap | Min Window, Longest Substring |
| Searching & Sorting | Binary Search, Rotated Array, Merge Intervals |
| Two Pointers | 3Sum, Container, Trapping Rain Water |
| Greedy & Bit Manipulation | Jump Game, XOR tricks |

---

**End of Guide** — Happy revising! 🚀