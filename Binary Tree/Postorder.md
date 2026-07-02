---
type: concept
tags: [tree, binary-tree, dfs, recursion, cpp, math]
date: 2026-06-30
---
# Postorder Traversal

## Mathematical Definition
**Postorder Traversal** is the rigorous terminal evaluation of a geometric binary tree, following the strict bottom-up evaluation topology:
1. Penetrate and evaluate the entire left subtree $T_L$.
2. Penetrate and evaluate the entire right subtree $T_R$.
3. Geometrically isolate and evaluate the structural Root node $r$.

Postorder guarantees that a distinct parent is strictly never processed until all of its absolute structural descendants have fundamentally terminated. This mathematically underpins operations like deleting a tree memory footprint (freeing children before parents) and algebraic postfix evaluation.

---

## Recursive Approach

```cpp
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void postorderHelper(TreeNode* node, vector<int>& res) {
    if (node == nullptr) return; // Structural base-bound
    
    postorderHelper(node->left, res);   // 1. Fully resolve Left subspace
    postorderHelper(node->right, res);  // 2. Fully resolve Right subspace
    res.push_back(node->val);           // 3. Resolve parent dynamically
}

vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    postorderHelper(root, res);
    return res;
}
```

---

## Iterative Approach (Explicit Stack)

Iterative Postorder is structurally the most complex DFS model. A node must remain bounded on the stack until *both* its left and right sequences are resolved. 

We can theoretically map this to a Dual-Stack model, or an inverse modification of Preorder (Root $\to$ Right $\to$ Left, followed by a total sequence inversion).

**Modified Sequence Inversion (Theoretical Simulation):**
```cpp
#include <vector>
#include <stack>
#include <algorithm>

using namespace std;

vector<int> iterativePostorder(TreeNode* root) {
    vector<int> res;
    if (root == nullptr) return res;
    
    stack<TreeNode*> s;
    s.push(root);
    
    // Evaluate pseudo-Preorder sequence: Root -> Right -> Left
    while (!s.empty()) {
        TreeNode* current = s.top();
        s.pop();
        
        res.push_back(current->val);
        
        // Reversed spatial push vs strict Preorder
        if (current->left != nullptr) s.push(current->left);
        if (current->right != nullptr) s.push(current->right);
    }
    
    // Symmetrically invert the spatial vector to achieve Left -> Right -> Root
    reverse(res.begin(), res.end());
    
    return res;
}
```

> [!warning]
> While the sequence inversion geometrically outputs correctly in $\mathcal{O}(N)$ time, it does not strictly emulate true bottom-up processing during execution. If the processing operation mathematically requires true bottom-up guarantees (e.g., aggregating sub-tree structural counts live), a rigorous Single-Stack geometric backtracking evaluation with a `lastVisited` pointer must be deployed instead.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. The tree is mathematically penetrated iteratively exactly once. The sequence inversion is strictly bounded by $\mathcal{O}(N)$.
- **Space Complexity:** $\mathcal{O}(H)$, dynamically tracking spatial boundaries.
