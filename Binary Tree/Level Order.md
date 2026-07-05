# Level Order Traversal

## Mathematical Definition
- **level Order Traversal** executes a strict Breadth-First Search (BFS) topology across a geometric graph. It evaluates nodes chronologically sorted by their exact absolute structural depth $d$ from the primary root. All nodes resting precisely at $d = k$ are mathematically evaluated prior to any node residing at $d = k+1$.


## Approach: FIFO Queue Invariant

- depth-First structures depend fundamentally on LIFO topologies (Stacks). Conversely, BFS mandates theoretical fairness and temporal ordering based strictly on distance, mandating a FIFO Queue `std::queue`.

- algorithm:
- enqueue the absolute geometric root $r$.
- initiate a standard loop constrained while the queue theoretically maintains elements.
- for exact multi-level delineation, dynamically measure the spatial queue size $S$ prior to popping.
- structurally pop exactly $S$ contiguous elements. These elements constitute the precise geometry of depth $d$.
- evaluate the popped values, and subsequently enqueue their immediate left and right valid geometric children.


## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (root == nullptr) return res;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int level_size = q.size(); // Rigid constraint for current depth geometry
        vector<int> current_level;
        
        for (int i = 0; i < level_size; i++) {
            TreeNode* current = q.front();
            q.pop();
            
            current_level.push_back(current->val); // Evaluate temporal node
            
            // Push subsequent spatial depth bounds
            if (current->left != nullptr) q.push(current->left);
            if (current->right != nullptr) q.push(current->right);
        }
        
        res.push_back(current_level); // Record discrete depth slice
    }
    
    return res;
}
```

> [!tip]
> Pre-allocating `current_level.reserve(level_size)` theoretically bypasses internal vector heap reallocations, strictly guaranteeing optimized $\mathcal{O}(1)$ temporal insertions across the level boundary.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Every geometrical node mathematically enters and exits the FIFO structure exclusively once.
- **space Complexity:** $\mathcal{O}(W)$, where $W$ is the absolute maximum structural width of the tree. For a perfectly bounded full binary tree, $W$ at the final temporal leaf layer evaluates to $\approx \frac{N}{2}$, rendering the space bound structurally $\mathcal{O}(N)$.

NEXT: [[Index]]
