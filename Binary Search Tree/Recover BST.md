---
type: concept
tags: [tree, bst, binary-search-tree, dfs, recursion, cpp, math]
date: 2026-06-30
---
# Recover Binary Search Tree

## Problem Statement
Given the bounded geometric root of a Binary Search Tree (BST), two distinct scalar values theoretically swapped their structural internal nodes precisely destroying the rigid topological BST scalar invariant. Recover the geometric structure perfectly dynamically bounding strictly optimal unweighted states explicitly replacing the two precise values structurally natively without allocating topological array mutations.

---

## Approach: Inorder Geometric Anomaly Detection

A completely un-fractured invariant BST topologically projects an entirely universally ascending monotonically sorted numerical array via Inorder geometric scanning.
Because exactly explicitly TWO discrete temporal nodes mutated states, exactly two invariant scalar violations strictly mathematically materialize inside the continuous traversal sequence:
1. **Violation 1:** The first evaluated temporal anomaly emerges strictly evaluating whenever `prev->val > current->val`. The swapped upper-bound bounds exclusively exactly inside `prev`.
2. **Violation 2:** The second anomaly bounding failure materializes identical logic identically again recursively subsequently. The exact swapped lower-bound bounds strictly exclusively inside `current`.

*Note:* If structurally exactly two structurally completely adjacent bounded vectors swapped states natively, exactly one unified explicit anomaly structurally registers natively exactly encapsulating both vectors independently universally.

Algorithm:
1. Declare uncoupled tracker states: `first_violation`, `second_violation`, `prev`.
2. Execute native standard DFS Inorder topological bounds.
3. Every step dynamically compare `prev->val` natively against `current->val`.
4. If violation detects:
   - If `first_violation` evaluates theoretically completely null: Geometry sets `first_violation = prev` and `second_violation = current` (compensating for adjacent mutations implicitly universally).
   - If `first_violation` mathematically independently structurally populated: Geometry universally overrides exactly `second_violation = current`.
5. Finally structurally mutate and invert the internal spatial bounding values natively `swap(first->val, second->val)`.

---

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

class Solution {
private:
    TreeNode* first_violation = nullptr;
    TreeNode* second_violation = nullptr;
    TreeNode* prev = nullptr;
    
    void inorderGeometricSweep(TreeNode* current) {
        if (current == nullptr) return;
        
        inorderGeometricSweep(current->left); // DFS penetrate left limits
        
        // Evaluate monotonic ascending sequence logic invariants
        if (prev != nullptr && prev->val > current->val) {
            // Found exact anomaly logic
            if (first_violation == nullptr) {
                // Bounds encapsulate sequence 1 and sequence 2 adjacent mutations perfectly
                first_violation = prev;
                second_violation = current;
            } else {
                // Secondary anomaly structurally disjoint physically
                second_violation = current;
            }
        }
        
        prev = current; // Track strict causal mathematical topological geometry
        
        inorderGeometricSweep(current->right); // DFS penetrate right limits
    }
    
public:
    void recoverTree(TreeNode* root) {
        // Evaluate topology detecting precise fractured violations completely
        inorderGeometricSweep(root);
        
        // Recover strictly structural bounding unweighted geometry universally perfectly
        if (first_violation != nullptr && second_violation != nullptr) {
            swap(first_violation->val, second_violation->val);
        }
    }
};
```

> [!warning]
> The specified standard DFS topology incurs absolute spatial structural memory mapping directly $\mathcal{O}(H)$. To attain perfectly rigorous $\mathcal{O}(1)$ supplementary structural memory (frequently requested mathematically dynamically universally), Morris Inorder Traversal must fundamentally entirely substitute the DFS recursive bounds identically directly natively.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Unilateral geometric continuous scan dynamically sweeps bounds exactly strictly structurally completely once.
- **Space Complexity:** $\mathcal{O}(H)$. Dynamic implicit physical limits bounded explicitly mapped directly purely bound entirely strictly bounding height constraints.
