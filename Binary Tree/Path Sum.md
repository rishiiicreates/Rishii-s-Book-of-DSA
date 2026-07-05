# Path Sum (Root to Leaf)

## Problem Statement
- given the root of a binary tree and an integer `targetSum`, mathematically determine if the tree possesses a strict **root-to-leaf** path such that adding up all the node values along the path yields exactly `targetSum`.

- *definition:* A leaf is a node with no topological children (i.e., both left and right pointers are exactly `nullptr`).


## Approach: Depth-First Search (DFS) with Sum Subtraction

- the most elegant mathematical reduction involves verifying the target sum dynamically as we traverse down the tree using [[Depth First Search]]. Instead of accumulating a running sum and comparing it at the leaf, we subtract the current node's value from the `targetSum`.

- **null Base Case:** If the current node is `nullptr`, the path evaluates to false (an empty path cannot yield a sum).
- **state Modification:** Subtract the current node's value from the active `targetSum`.
   - $\text{targetSum}_{\text{new}} = \text{targetSum}_{\text{old}} - \text{node.data}$
- **leaf Verification Constraint:** If the current node is unequivocally a leaf node (`node->left == nullptr` AND `node->right == nullptr`), check if the mathematically adjusted $\text{targetSum}_{\text{new}} == 0$.
   - if it is exactly $0$, a valid path exists. Return `true`.
   - otherwise, return `false`.
- **recursive Exploration:** If the node is not a leaf, propagate the modified $\text{targetSum}_{\text{new}}$ down to its topological children via logical OR.
   - return `hasPathSum(node->left) || hasPathSum(node->right)`.


## Code Implementation

```cpp
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

bool hasPathSum(Node* root, int targetSum) {
    // Empty subtrees cannot complete a path sum
    if (root == nullptr) {
        return false;
    }
    
    // Deduct the current node's mathematical weight
    targetSum -= root->data;
    
    // Verify leaf node condition
    if (root->left == nullptr && root->right == nullptr) {
        return targetSum == 0;
    }
    
    // Recursively evaluate children (Top-Down Pre-Order)
    return hasPathSum(root->left, targetSum) || hasPathSum(root->right, targetSum);
}
```

> [!warning]
> A common pitfall is returning `targetSum == 0` when `root == nullptr`. This violates the strict definition of a root-to-leaf path. If a node has a left child but no right child, traversing the empty right child should **not** inadvertently validate a path. Leaf verification *must* happen distinctly at the parent node before null-pointer traversal.


## Complexity Analysis
- **time Complexity:** $O(N)$. In the worst-case scenario (no valid path exists), the algorithm must mathematically verify every single node in the tree structure exactly once.
- **space Complexity:** $O(H)$, where $H$ is the topological height of the tree. The implicit recursion stack utilizes memory proportional to the length of the longest path.

NEXT: [[Index]]
