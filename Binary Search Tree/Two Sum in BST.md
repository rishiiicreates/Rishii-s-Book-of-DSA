# Two Sum in BST

## Problem Statement
- given the geometric root of a bounded Binary Search Tree (BST) alongside a strict scalar $K$, evaluate theoretically whether exactly two discrete nodes spatially physically reside within the topology whose absolute sum mathematically satisfies precisely $V_1 + V_2 = K$.


## Approach: Dual Stack Iterative Geometric Iterators (BST Iterator)

- while an $\mathcal{O}(N)$ spatial serialization of the structure directly mapping to a linear array natively solves the invariant identically identical to standard "Two Sum II" (Sorted Arrays), allocating absolute $\mathcal{O}(N)$ memory dictates inefficiencies.
- we dynamically strictly project theoretical two-pointers structurally natively atop the topological geometry bounded exclusively strictly at $\mathcal{O}(H)$ memory.

- we instantiate exactly two iterative geometric traversals:
- `forward Iterator` sequentially simulating standard Inorder (Left $\rightarrow$ Root $\rightarrow$ Right).
- `backward Iterator` sequentially simulating Reverse Inorder (Right $\rightarrow$ Root $\rightarrow$ Left).

- algorithm:
- two explicit uncoupled stacks dictate topological limits: `stack_forward` geometrically tracks the smallest absolute bounds, `stack_backward` tracks the largest absolute bounds.
- iterative advancement geometrically behaves physically identically to linear two-pointer sequences:
   - if $V_{left} + V_{right} == K$: Geometry found, return true.
   - if $V_{left} + V_{right} < K$: The sum algebraically resides too small. Physically advance strictly the forward temporal sequence boundary geometrically to the next theoretical scalar.
   - if $V_{left} + V_{right} > K$: The sum algebraically resides too large. Physically decrement strictly the backward sequence boundary geometrically to the next theoretical lower scalar.
- if both boundaries physically temporally collide algebraically, geometry evaluates false.


## Code Implementation

```cpp
#include <stack>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class BSTIterator {
private:
    stack<TreeNode*> st;
    bool forward; // Flag dictating topological geometric traversal orientation
    
    // Push bounding extreme paths linearly bounded completely to O(H) depth
    void pushAll(TreeNode* node) {
        while (node != nullptr) {
            st.push(node);
            node = (forward == true) ? node->left : node->right;
        }
    }

public:
    BSTIterator(TreeNode* root, bool isForward) : forward(isForward) {
        pushAll(root); // Initialize limits
    }
    
    // Evaluate spatial state and geometrically advance boundaries
    int next() {
        TreeNode* current = st.top();
        st.pop();
        
        // Push contrasting bounded limit to mimic topological recursive causality
        if (forward) {
            pushAll(current->right);
        } else {
            pushAll(current->left);
        }
        
        return current->val;
    }
};

bool findTarget(TreeNode* root, int k) {
    if (root == nullptr) return false;
    
    BSTIterator left_ptr(root, true);   // Simulates Array L pointer
    BSTIterator right_ptr(root, false); // Simulates Array R pointer
    
    int i = left_ptr.next();
    int j = right_ptr.next();
    
    while (i < j) {
        if (i + j == k) {
            return true;
        } else if (i + j < k) {
            i = left_ptr.next();
        } else {
            j = right_ptr.next();
        }
    }
    
    return false;
}
```

> [!important]
> The explicitly uncoupled iteration logic fundamentally maps theoretical recursion structures entirely decoupled from recursive spatial limitations, allowing the process to mathematically exit optimally significantly before scanning exactly $\mathcal{O}(N)$ geometries entirely.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$ worst-case. In strictly pathological scenarios completely devoid of pairs, topological iteration maps exactly through bounded structures exactly once.
- **space Complexity:** $\mathcal{O}(H)$. Pure uncoupled physical iterative stacks restrict limits universally precisely bounded exclusively bounded equal directly to height $H$.

NEXT: [[Index]]
