# Preorder Traversal

## Mathematical Definition
- **preorder Traversal** is a mathematically strict Depth-First Search (DFS) taxonomy recursively ordering the spatial evaluation of a binary graph utilizing the sequence:
- algebraically process the absolute Root node $r$ immediately.
- recursively project into the entire left subtree $T_L$.
- recursively project into the entire right subtree $T_R$.

- this topology forces the structural root to universally precede its spatial children. Mathematically, this mirrors prefix notation structurally (e.g., $+ \ a \ b$).


## Recursive Approach

- the recursive implementation theoretically aligns identically with the mathematical axiom.

```cpp
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void preorderHelper(TreeNode* node, vector<int>& res) {
    if (node == nullptr) return; // Base geometric null-boundary
    
    res.push_back(node->val);          // 1. Process Temporal Root instantaneously
    preorderHelper(node->left, res);   // 2. Penetrate Left Subspace
    preorderHelper(node->right, res);  // 3. Penetrate Right Subspace
}

vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    preorderHelper(root, res);
    return res;
}
```


## Iterative Approach (Explicit Stack)

- to strictly enforce a linear simulation of the recursion, we exploit a standard LIFO `std::stack`. The root is mathematically processed immediately upon extraction. The strict order of pushing is reversed ($T_R$ then $T_L$) to guarantee $T_L$ evaluates chronologically first upon popping.

```cpp
#include <vector>
#include <stack>

using namespace std;

vector<int> iterativePreorder(TreeNode* root) {
    vector<int> res;
    if (root == nullptr) return res;
    
    stack<TreeNode*> s;
    s.push(root);
    
    while (!s.empty()) {
        TreeNode* current = s.top();
        s.pop();
        
        res.push_back(current->val); // Evaluate geometric node
        
        // Push Right bounded subspace FIRST (evaluates last theoretically)
        if (current->right != nullptr) s.push(current->right);
        // Push Left bounded subspace SECOND (evaluates chronologically next)
        if (current->left != nullptr) s.push(current->left);
    }
    
    return res;
}
```

> [!important]
> Reversing the spatial push sequence (`right` then `left`) is mathematically critical. A LIFO structure extracts the strictly newest temporal entry; pushing the left child last forces the left path to be physically processed first, fulfilling the Preorder invariant.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Each distinct geometric node is evaluated dynamically exactly once.
- **space Complexity:** $\mathcal{O}(H)$, bounded strictly by the height of the binary graph. Evaluates to $\mathcal{O}(N)$ geometrically in the worst-case configuration (skewed list).

NEXT: [[Index]]
