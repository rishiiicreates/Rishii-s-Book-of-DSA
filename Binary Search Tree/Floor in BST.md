# Floor in a Binary Search Tree

## Problem Statement
- given the geometric root of a Binary Search Tree (BST) and an absolute scalar key $X$, mathematically compute the `floor` of $X$ within the tree.
- the mathematical floor is defined as the absolute maximum geometric vertex $V$ such that $V.\text{val} \le X$. If no such vertex exists, return a designated boundary indicator (e.g., `-1`).


## Approach: Monotonic Directed Search

- the strict topological definition of a BST guarantees $T_L.\text{val} < T.\text{val} < T_R.\text{val}$ for all valid subtrees.
- we execute a monotonic iterative descent:
- initialize a geometric tracker `floor_val = -1`.
- evaluate the current vertex $V$:
   - if $V.\text{val} == X$, absolute mathematical equivalence is met. $X$ is exactly its own floor. Return $V.\text{val}$.
   - if $V.\text{val} > X$, $V$ is mathematically out of bounds. The floor MUST strictly reside in the geometrically smaller left subtree. Shift $V = V \to \text{left}$.
   - if $V.\text{val} < X$, $V$ is a valid mathematical candidate for the floor. We log its scalar (`floor_val = V.val`) and attempt to find a *tighter* maximum bound by descending into the right subtree. Shift $V = V \to \text{right}$.


## Code Implementation

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int floorInBST(TreeNode* root, int X) {
    int floor_val = -1; // Negative infinity bound assumption
    
    // Iterative Monotonic Descent
    while (root != nullptr) {
        if (root->val == X) {
            // Absolute mathematical equivalence
            return root->val;
        }
        
        if (root->val > X) {
            // Root exceeds upper bound; geometric isolation to Left Subtree
            root = root->left;
        } else {
            // Root is a mathematically valid floor candidate
            floor_val = root->val;
            
            // Attempt to converge on a tighter maximum bound
            root = root->right;
        }
    }
    
    return floor_val;
}
```


## Complexity Analysis
- **time Complexity:** $O(\log N)$ expected bound, bounded by the geometric height $O(H)$ of the BST. In a strictly degenerate topology, degrades to $O(N)$.
- **space Complexity:** $O(1)$ absolute spatial mapping. The search is purely iterative and manipulates a single scalar tracker.

> [!tip]
> **Ceiling Symmetry:** The algorithm for calculating the `Ceil` ($V.\text{val} \ge X$) is perfectly symmetrical. If $V.\text{val} < X$, move right. If $V.\text{val} > X$, log $V$ as a ceiling candidate and monotonically descend left.

NEXT: [[Index]]
