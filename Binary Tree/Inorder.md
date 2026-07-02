---
type: concept
tags: [tree, binary-tree, dfs, recursion, cpp, math]
date: 2026-06-30
---
# Inorder Traversal

## Mathematical Definition
**Inorder Traversal** is a continuous, strictly defined Depth-First Search (DFS) algorithm recursively evaluating a geometric binary tree utilizing the spatial sequence:
1. Algebraically evaluate the entire left subtree $T_L$ in Inorder.
2. Algebraically process the absolute Root node $r$.
3. Algebraically evaluate the entire right subtree $T_R$ in Inorder.

For a Binary Search Tree (BST), the structural invariant enforces that all scalars in $T_L < r < T_R$. Thus, Inorder Traversal mathematically projects a BST into a strictly monotonic (sorted) linear sequence.

---

## Recursive Approach

The recursive mathematical invariant structurally mirrors the definition. We theoretically push the evaluation state to the System Call Stack until hitting the absolute geometric base case (the $\emptyset$ null set).

```cpp
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void inorderHelper(TreeNode* node, vector<int>& res) {
    if (node == nullptr) return; // Base geometric bound
    
    inorderHelper(node->left, res);   // 1. Penetrate Left Spatial Bound
    res.push_back(node->val);         // 2. Resolve Temporal Root
    inorderHelper(node->right, res);  // 3. Penetrate Right Spatial Bound
}

vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    inorderHelper(root, res);
    return res;
}
```

---

## Iterative Approach (Explicit Stack)

To bypass the overhead of structural system calls and prevent stack overflow anomalies in severely skewed degenerate trees ($H \approx N$), we explicitly simulate the LIFO progression.

```cpp
#include <vector>
#include <stack>

using namespace std;

vector<int> iterativeInorder(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> s;
    TreeNode* current = root;
    
    // Terminate strictly when spatial tracking is exhausted
    while (current != nullptr || !s.empty()) {
        // Greedily penetrate to the absolute leftmost boundary
        while (current != nullptr) {
            s.push(current);
            current = current->left;
        }
        
        // Resolve spatially deepest unvisited temporal node
        current = s.top();
        s.pop();
        res.push_back(current->val); // Process Root
        
        // Transition evaluation to the right subspace
        current = current->right;
    }
    
    return res;
}
```

> [!tip]
> Morris Traversal offers a theoretically superior strictly $\mathcal{O}(1)$ auxiliary space alternative by temporarily mutating the structural pointers of the binary tree to explicitly back-track without a stack, though it fundamentally mutates read-only graph structures during execution.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Each geometric node is pushed into the recursive or explicit stack strictly exactly once and popped strictly exactly once.
- **Space Complexity:** $\mathcal{O}(H)$, where $H$ defines the absolute geometric height of the tree structure. $H = \lceil \log_2(N) \rceil$ for perfectly balanced trees, and $H = N$ for degenerate structures.
