# Construct Binary Search Tree from Preorder Traversal

## Problem Statement
- given an array containing the absolute mathematical Preorder Traversal sequence of a Binary Search Tree (BST), reconstruct the exact original topological tree structure.


## Approach: Upper Bound Scalar Limitation (O(N))

- a naive mapping would utilize $O(N^2)$ scalar sorting or leverage In-Order reconstruction $O(N \log N)$.
- however, because Preorder traversal implies a strict sequence format `(Root, Left_Subtree, Right_Subtree)`, we can reconstruct the geometry in pure $O(N)$ linear time utilizing **Upper Bound Pointer Limits**.
- we maintain a global index pointer `i` traversing the Preorder array sequentially.
- for any arbitrary node construction:
- it is bound by an absolute theoretical maximum scalar limit (`bound`).
- if the current sequence scalar $A[i] > \text{bound}$, the topological placement is invalid, mapping a return `nullptr`.
- otherwise, consume the array element and construct the geometric root $V$.
- recursively construct $V$'s left subtree, updating the strict upper bound to $V.\text{val}$.
- recursively construct $V$'s right subtree, utilizing the original unmodified parent upper limit.


## Code Implementation

```cpp
#include <vector>
#include <climits>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* buildBST(vector<int>& preorder, int& i, int bound) {
    // Spatial Sequence Termination or Geometric Violation
    if (i == preorder.size() || preorder[i] > bound) {
        return nullptr;
    }
    
    // Vertex instantiation
    TreeNode* root = new TreeNode(preorder[i++]);
    
    // Left boundary constraint collapses strictly to Root scalar
    root->left = buildBST(preorder, i, root->val);
    
    // Right boundary constraint inherits the global absolute ceiling
    root->right = buildBST(preorder, i, bound);
    
    return root;
}

TreeNode* bstFromPreorder(vector<int>& preorder) {
    int i = 0;
    // Initialization bounded by theoretical infinity
    return buildBST(preorder, i, INT_MAX);
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ absolute limit. Every scalar in the Preorder sequence topological array is evaluated and structurally linked exactly once, executing strict $O(1)$ constant time comparisons.
- **space Complexity:** $O(H)$ where $H$ denotes the topological height mapping of the newly constructed Binary Search Tree.

> [!warning]
> **Redundant Sorting Isolation:** A common inefficient approach involves mapping a copy of the array, sorting it to artificially construct an In-Order traversal array, and processing dual-pointer topology. This immediately forces minimum $O(N \log N)$ mapping overhead. The Upper Bound approach mathematically out-scales it to true $O(N)$.

NEXT: [[Index]]
