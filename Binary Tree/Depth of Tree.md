# Depth (Height) of a Binary Tree

## Problem Statement
- given the geometric root node of a Binary Tree $T$, compute its absolute mathematical height, defined as the maximum topological edge-path distance from the root to any remote leaf vertex (often equivalently measured as the absolute maximum node depth).


## Approach: Recursive Max-Path Bounding

- let the height of a subtree $T$ be denoted as $H(T)$. Topologically, the height of a non-empty tree is exactly one geometric unit greater than the *maximum* height of its two disjoint structural child subtrees.
- this establishes the recurrence relation:
$$ H(T) = \begin{cases} 0 & \text{if } T = \emptyset \\ 1 + \max(H(T_L), H(T_R)) & \text{otherwise} \end{cases} $$

- we execute a geometric post-order DFS traversal. A node computes the absolute maximal depth of its disjoint left and right topological paths, and mathematically passes the bound $+1$ to its geometric parent.


## Code Implementation

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

// Topological Node Structure
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int maxDepth(TreeNode* root) {
    // Base Case Geometry
    if (root == nullptr) {
        return 0;
    }
    
    // Subtree Mathematical Bounds
    int left_height = maxDepth(root->left);
    int right_height = maxDepth(root->right);
    
    // Parent Geometrical Bound
    return 1 + max(left_height, right_height);
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ exact linear constraint, as every node must be topologically evaluated to certify the global maximum bound.
- **space Complexity:** $O(H)$ bounding the execution call-stack depth, mapping to $O(\log N)$ perfectly balanced, or $O(N)$ pathologically degenerate.

> [!important]
> **Depth vs. Height Semantics:** Strictly mathematically, *Depth* is measured continuously downwards from the root ($0$ at root, increasing), while *Height* is measured upwards from the deepest leaf ($0$ at leaf, increasing). However, the absolute maximum Depth of a tree is topologically identical to its absolute Height.

NEXT: [[Index]]
