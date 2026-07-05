# Invert Binary Tree

## Problem Statement
- given the topological root of a Binary Tree $T$, mathematically compute its absolute mirror reflection across its central vertical geometric axis. The resulting tree is strictly the inverted image of the original topology.


## Approach: Recursive Geometric Reflection (DFS)

- topological inversion is a mathematically pure recursive operation. A Binary Tree $T$ is inverted if and only if:
- the left subtree $T_L$ is geometrically swapped with the right subtree $T_R$.
- both $T_L$ and $T_R$ are themselves structurally inverted.

- this maps perfectly to a Depth-First Search (DFS) operation. At every distinct vertex $v$, we atomically execute a scalar swap of the `left` and `right` child pointers, and inductively propagate the inversion operation down the structural height of the tree. The base case bounds on the $\emptyset$ (null) terminal condition.


## Code Implementation

```cpp
#include <utility>

using namespace std;

// Topological Node Structure
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* invertTree(TreeNode* root) {
    // Geometric Base Case
    if (root == nullptr) {
        return nullptr;
    }
    
    // Atomic Pointer Isomorphism (Reflection)
    swap(root->left, root->right);
    
    // Inductive Structural Propagation
    invertTree(root->left);
    invertTree(root->right);
    
    return root;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ strict linear traversal. The algorithm mathematically visits every structural vertex exactly once to execute the geometric swap.
- **space Complexity:** $O(H)$ mapping the recursive depth bound of the call-stack, peaking at $O(N)$ for a strictly degenerate sequence, and $O(\log N)$ for a perfectly balanced topology.

> [!tip]
> **Iterative Isomorphism:** This topological mutation is not exclusively bound to DFS. A Breadth-First Search (BFS) utilizing a `std::queue` achieves identical geometric reflection iteratively, evaluating pointer swaps perfectly level-by-level, effectively neutralizing call-stack depth limits.

NEXT: [[Index]]
