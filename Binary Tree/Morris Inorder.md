# Morris Inorder Traversal

## Problem Statement
- mathematically evaluate the geometric Inorder sequence of a Binary Tree achieving strictly linear $\mathcal{O}(N)$ temporal performance whilst utilizing an absolutely constrained $\mathcal{O}(1)$ supplementary structural space, entirely avoiding both explicit Data Stacks and implicit System Call Stacks.


## Approach: Structural Threading (Morris Traversal)

- a standard [[Inorder]] execution necessitates $\mathcal{O}(H)$ spatial memory to theoretically re-evaluate the parent after structurally resolving the left child subspace.
- morris Traversal bypasses this constraint by dynamically threading the binary graph: it mathematically restructures the read-only $\emptyset$ bounds (null pointers) of terminal leaves to explicitly point temporally backward to their Inorder successor.

- algorithm:
- initialize `current` at the absolute root.
- if `current->left` evaluates strictly to null, the local left subspace is empty. Evaluate `current->val` and structurally transition right: `current = current->right`.
- if a left subspace mathematically exists, geometrically discover the **Inorder Predecessor** (the structurally absolute rightmost node bounded within the left subtree).
- if the Predecessor's right bound is null:
   - temporally thread it to `current` (`pre->right = current`).
   - penetrate deeper left: `current = current->left`.
- if the Predecessor's right bound already explicitly points to `current`, the threading is fully evaluated.
   - sever the thread to strictly restore original graph topology (`pre->right = nullptr`).
   - evaluate `current->val`.
   - transition right: `current = current->right`.


## Code Implementation

```cpp
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

vector<int> morrisInorderTraversal(TreeNode* root) {
    vector<int> res;
    TreeNode* current = root;
    
    while (current != nullptr) {
        if (current->left == nullptr) {
            // No structural predecessor subspace; evaluate directly
            res.push_back(current->val);
            current = current->right;
        } else {
            // Geometrically locate absolute Inorder Predecessor
            TreeNode* pre = current->left;
            while (pre->right != nullptr && pre->right != current) {
                pre = pre->right;
            }
            
            // Thread creation: Establish temporal back-link
            if (pre->right == nullptr) {
                pre->right = current;
                current = current->left;
            } 
            // Thread evaluation: Re-link destruction and spatial continuation
            else {
                pre->right = nullptr; // Mathematically restore invariant
                res.push_back(current->val);
                current = current->right;
            }
        }
    }
    
    return res;
}
```

> [!warning]
> Morris Traversal actively mutates graph topology during execution. If multiple concurrent threads execute this algorithm on a shared tree instance, massive synchronization collisions and cycle loops are structurally guaranteed.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Although locating the theoretical predecessor mandates a nested sequence, each edge in the entire geometry is traversed structurally exactly twice (once to thread, once to evaluate). Thus, performance is strictly $2N \implies \mathcal{O}(N)$.
- **space Complexity:** $\mathcal{O}(1)$ supplementary structure. Bypasses the call stack completely utilizing explicit pointer mutation.

NEXT: [[Index]]
