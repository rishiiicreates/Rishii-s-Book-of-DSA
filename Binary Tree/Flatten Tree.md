# Flatten Binary Tree to Linked List

## Problem Statement
- given a generic Binary Tree topology $T$, mathematically flatten it *in-place* into a strictly degenerate right-skewed sequential structure, which isomorphicly mirrors a Singly Linked List. The topological sequence must strictly conform exactly to a standard Pre-Order Depth-First Traversal mapping.


## Approach: Morris Traversal (O(1) Spatial Topology)

- a naive recursive implementation executes a Pre-Order DFS, caching nodes in an $O(N)$ spatial array, and reconstructs the bounds linearly. Alternatively, an $O(H)$ recursion maps right bounds first. Both violate absolute $O(1)$ spatial perfection.

- to achieve strict $O(1)$ auxiliary memory, we execute a **Morris Traversal** structural manipulation.
- for any vertex $v$ with a left child $T_L$:
- the mathematically last node evaluated in $T_L$'s Pre-Order traversal is explicitly its absolute rightmost geometric descendant $v_{R\_max}$.
- the sequence dictates that $v_{R\_max}$ must topological point to $v$'s original right subtree $T_R$.
- we architect a temporary topological bridge: map $v_{R\_max} \to \text{right} = v \to \text{right}$.
- we surgically shift the entire left bound: map $v \to \text{right} = v \to \text{left}$, and irrevocably sever $v \to \text{left} = \emptyset$.
- advance the cursor strictly to the geometric right and repeat.


## Code Implementation

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void flatten(TreeNode* root) {
    TreeNode* current = root;
    
    // Iterative Topological Deformation
    while (current != nullptr) {
        if (current->left != nullptr) {
            // Locate the extreme geometric right vertex of the left disjoint set
            TreeNode* right_most = current->left;
            while (right_most->right != nullptr) {
                right_most = right_most->right;
            }
            
            // Bridge the topology
            right_most->right = current->right;
            
            // Re-anchor the parent structural bound
            current->right = current->left;
            current->left = nullptr; // Sever left parity
        }
        
        // Unidirectional temporal advance
        current = current->right;
    }
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ amortized constraint. While the inner `while` loop hunts the geometric boundary, across the global execution, each structural edge is traversed structurally at most twice.
- **space Complexity:** $O(1)$ mathematically absolute bound. Zero dynamic allocations or recursive stack frames exist.

> [!important]
> **Pre-Order Preservation:** The Morris topological bridge mathematically forces the traversal to naturally serialize into Pre-Order. The left child immediately succeeds the parent, and the orphaned right child is deferred seamlessly to the absolute tail of the left subset.

NEXT: [[Index]]
