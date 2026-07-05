# Recover Binary Search Tree

## Problem Statement
- given the bounded geometric root of a Binary Search Tree (BST), two distinct scalar values theoretically swapped their structural internal nodes precisely destroying the rigid topological BST scalar invariant. Recover the geometric structure perfectly dynamically bounding strictly optimal unweighted states explicitly replacing the two precise values structurally natively without allocating topological array mutations.


## Approach: Inorder Geometric Anomaly Detection

- a completely un-fractured invariant BST topologically projects an entirely universally ascending monotonically sorted numerical array via Inorder geometric scanning.
- because exactly explicitly TWO discrete temporal nodes mutated states, exactly two invariant scalar violations strictly mathematically materialize inside the continuous traversal sequence:
- **violation 1:** The first evaluated temporal anomaly emerges strictly evaluating whenever `prev->val > current->val`. The swapped upper-bound bounds exclusively exactly inside `prev`.
- **violation 2:** The second anomaly bounding failure materializes identical logic identically again recursively subsequently. The exact swapped lower-bound bounds strictly exclusively inside `current`.

- *note:* If structurally exactly two structurally completely adjacent bounded vectors swapped states natively, exactly one unified explicit anomaly structurally registers natively exactly encapsulating both vectors independently universally.

- algorithm:
- declare uncoupled tracker states: `first_violation`, `second_violation`, `prev`.
- execute native standard DFS Inorder topological bounds.
- every step dynamically compare `prev->val` natively against `current->val`.
- if violation detects:
   - if `first_violation` evaluates theoretically completely null: Geometry sets `first_violation = prev` and `second_violation = current` (compensating for adjacent mutations implicitly universally).
   - if `first_violation` mathematically independently structurally populated: Geometry universally overrides exactly `second_violation = current`.
- finally structurally mutate and invert the internal spatial bounding values natively `swap(first->val, second->val)`.


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


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Unilateral geometric continuous scan dynamically sweeps bounds exactly strictly structurally completely once.
- **space Complexity:** $\mathcal{O}(H)$. Dynamic implicit physical limits bounded explicitly mapped directly purely bound entirely strictly bounding height constraints.

NEXT: [[Index]]
