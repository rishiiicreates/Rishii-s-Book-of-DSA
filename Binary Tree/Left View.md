# Left View of Binary Tree

## Problem Statement
- given a Binary Tree, mathematically compute its **Left View**. The left view of a binary tree is defined as the ordered set of nodes visible when the tree is observed strictly from its left side. Equivalently, it is the first node encountered at each discrete depth level.


## Approach: Depth-First Search (DFS) with Level Tracking

- while [[Level Order Traversal]] (BFS) easily yields the first node of each level, applying a [[Pre-Order Traversal]] (DFS) optimized with depth-tracking provides an elegant and slightly more memory-efficient solution (failing recursion stack vs. queue footprint).

- **state Invariant:** We maintain a variable `max_level_reached` initialized to $0$. We pass the `current_level` (1-indexed) into our recursive DFS function.
- **pre-Order Traversal:** We prioritize exploring the left child before the right child. This mathematical constraint guarantees that the first time the DFS reaches a specific level $L$, it is unconditionally visiting the leftmost node of that level.
- **observation Trigger:** When we visit a node at `current_level`, we check if `current_level > max_level_reached`.
   - if true, this is the first mathematical observation of this depth. We record the node's value and update `max_level_reached = current_level`.
- **recursion:**
   - traverse `node->left` with `current_level + 1`.
   - traverse `node->right` with `current_level + 1`.


## Code Implementation

```cpp
#include <vector>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

void leftViewUtil(Node* root, int current_level, int& max_level_reached, vector<int>& res) {
    if (root == nullptr) return;
    
    // First time reaching this depth level
    if (current_level > max_level_reached) {
        res.push_back(root->data);
        max_level_reached = current_level;
    }
    
    // Strictly Left-First Traversal (Pre-Order variant)
    leftViewUtil(root->left, current_level + 1, max_level_reached, res);
    leftViewUtil(root->right, current_level + 1, max_level_reached, res);
}

vector<int> leftView(Node* root) {
    vector<int> res;
    int max_level_reached = 0;
    
    // Start DFS at depth level 1
    leftViewUtil(root, 1, max_level_reached, res);
    
    return res;
}
```

> [!important]
> If a specific level contains no nodes on the left topological branch (e.g., the left child of the root is missing), the recursive DFS naturally processes the right branch at `current_level + 1`, successfully capturing the leftmost visible node of that right branch. 


## Complexity Analysis
- **time Complexity:** $O(N)$. The DFS traverses every node exactly once to verify its level constraint.
- **space Complexity:** $O(H)$, where $H$ is the topological height of the tree. This accounts for the recursive call stack depth. In the worst case (a skewed tree), it reduces to $O(N)$.

NEXT: [[Index]]
