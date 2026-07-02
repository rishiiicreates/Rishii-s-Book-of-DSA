---
type: concept
tags: [binary-tree, bfs, pointer-manipulation, cpp]
date: 2026-06-30
---
# Connect Nodes at Same Level

## Problem Statement
Given a binary tree where every node possesses an additional auxiliary pointer called `nextRight` (initialized to `nullptr`), mathematically populate all `nextRight` pointers such that they point to the node strictly to their immediate right on the same topological depth level. The `nextRight` pointer of the absolute rightmost node at any level must permanently remain `nullptr`.

---

## Approach: Level Order Traversal (BFS)

The requirement explicitly mandates establishing lateral topological relationships across identically indexed depth tiers. This is structurally perfectly aligned with a [[Breadth First Search]] (Level Order Traversal) using a queue.

1. **Queue Instantiation:** Initialize a `std::queue<Node*>` and enqueue the `root`.
2. **Level Boundary Evaluation:** For each discrete level, the mathematical size of the queue represents the exact quantity of nodes occupying that specific level. Let this be `level_size`.
3. **Lateral Binding:** 
   - Iterate exactly `level_size` times.
   - Dequeue a `curr` node.
   - If this is **not** the absolute last node of the current iterative loop (`i < level_size - 1`), set `curr->nextRight = queue.front()`.
   - Enqueue the standard topological children (`curr->left` and `curr->right`).
4. **Sentinel Termination:** The last node inherently defaults to `nullptr` (assuming node constructors initialize `nextRight` properly), satisfying the boundary condition constraint.

### Optimization (Constant Space)
If the mathematical constraints demand $O(1)$ space, the algorithm can fundamentally rely on the previously established `nextRight` pointers acting as a custom linked list for the current level, using it to populate the child level directly without a dynamic queue.

---

## Code Implementation (Queue Based)

```cpp
#include <queue>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node* nextRight;
    Node(int val) : data(val), left(nullptr), right(nullptr), nextRight(nullptr) {}
};

void connect(Node* root) {
    if (root == nullptr) return;
    
    queue<Node*> q;
    q.push(root);
    
    while (!q.empty()) {
        int level_size = q.size();
        
        for (int i = 0; i < level_size; ++i) {
            Node* curr = q.front();
            q.pop();
            
            // Mathematically bind to the adjacent node in the queue
            // Unless this is the final topological node on the current level
            if (i < level_size - 1) {
                curr->nextRight = q.front();
            }
            
            // Queue subsequent hierarchical level
            if (curr->left) q.push(curr->left);
            if (curr->right) q.push(curr->right);
        }
    }
}
```

> [!important]
> Forgetting to cache `level_size` into a static scalar before initiating the `for` loop is a catastrophic logical error. The queue `size()` dynamically alters as children are enqueued, which would completely corrupt the lateral level boundaries.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. Every node in the binary tree is enqueued precisely once and dequeued precisely once. Link assignments compute in strictly $O(1)$ time.
- **Space Complexity:** $O(W)$, where $W$ is the maximum width of the binary tree. For a perfectly balanced tree, the deepest level holds $\frac{N}{2}$ nodes, yielding a strict worst-case boundary of $O(N)$ dynamic memory auxiliary overhead.
