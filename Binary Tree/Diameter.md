# Diameter of a Binary Tree

## Problem Statement
- mathematically define and compute the structural diameter of a binary tree. The diameter $D$ evaluates to the geometric length of the absolutely longest contiguous path bridging any two distinct nodes. This path may structurally arch entirely avoiding the mathematical root node. The length is strictly bounded by the number of continuous geometric *edges* (paths) linking them.


## Approach: Subtree Height Evaluation (DFS)

- geometrically, any continuous path arching strictly through a distinct parent node $U$ bounded connecting its left subtree to its right subtree holds an algebraic length evaluated as:
$$ \text{Path}(U) = \text{Height}(U_L) + \text{Height}(U_R) $$
- where $\text{Height}$ is the exact maximum temporal depth extending downwards.

- algorithm:
- construct a postorder DFS bounding the structural geometric height of any given subtree.
- for an arbitrary node $U$, evaluate the recursive geometric heights of its left and right subtrees: `lh` and `rh`.
- the absolute local diameter bridging structurally *through* $U$ calculates mathematically to `lh + rh`.
- maintain a rigorous global variable maximizing over all theoretically constructed local diameters.
- the DFS physically returns the structural height of $U$ itself back to its parent: `1 + max(lh, rh)`.


## Code Implementation

```cpp
#include <algorithm>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int diameterHelper(TreeNode* node, int& global_diameter) {
    if (node == nullptr) return 0; // The theoretical height of a null set is 0
    
    // Postorder penetration to calculate absolute spatial heights
    int left_height = diameterHelper(node->left, global_diameter);
    int right_height = diameterHelper(node->right, global_diameter);
    
    // Evaluate spatial arching bridge directly through the current topology
    global_diameter = max(global_diameter, left_height + right_height);
    
    // Return the continuous maximal height of the current geometric structure
    return 1 + max(left_height, right_height);
}

int diameterOfBinaryTree(TreeNode* root) {
    int global_diameter = 0;
    diameterHelper(root, global_diameter);
    return global_diameter; // Explicitly represents edges, not nodes
}
```

> [!important]
> Some variants theoretically define the diameter by the absolute number of *nodes* on the longest path. For nodes, the local arch structurally computes to `lh + rh + 1`. By canonical graph conventions (e.g., LeetCode 543), length is mathematically measured in discrete *edges*, necessitating simply `lh + rh`.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Subtree heights and arch diameters are mathematically evaluated continuously via a rigorous single $\mathcal{O}(N)$ DFS sweep, completely preventing the redundant $\mathcal{O}(N^2)$ traversal anomaly triggered by calling `height()` discretely on every node.
- **space Complexity:** $\mathcal{O}(H)$ bound on theoretical runtime stack frames dictating temporal descent depth.

NEXT: [[Index]]
