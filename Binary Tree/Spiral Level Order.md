---
type: concept
tags: [tree, binary-tree, bfs, queue, deque, cpp, math]
date: 2026-06-30
---
# Spiral (Zig-Zag) Level Order Traversal

## Problem Statement
Mathematically evaluate a Breadth-First Search (BFS) topology across a geometric binary tree structure, executing a strict sequence inversion uniformly alternating its directional spatial progression per depth layer $d$. At $d=1$, traverse left-to-right. At $d=2$, traverse right-to-left. 

---

## Approach: Alternating Depth Bounds via Deque

While canonical BFS dictates strict FIFO behavior, reversing alternate structural layers requires algorithmic divergence. To strictly maintain $\mathcal{O}(N)$ without invoking external $\mathcal{O}(N)$ `std::reverse` inversions iteratively on every spatial level vector, we substitute the standard FIFO boundary for a Double-Ended Queue (Deque).

Algorithm:
1. Initialize a `std::deque` representing the topological BFS layer.
2. Track a strict boolean spatial orientation flag `left_to_right = true`.
3. Geometrically iterate through each contiguous depth boundary $d$.
4. **Insertion Topology:** Instead of strictly pushing rightward on the evaluated result vector, leverage an internal `deque<int>` (or pre-allocated vector) for the specific layer state.
   - If `left_to_right == true`: Geometrically append the scalars standardly.
   - If `left_to_right == false`: Topologically unshift (push to the front) the scalars into the layer bounds.
5. Invert the logical sequence flag exclusively upon completing spatial boundary $d$.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
#include <deque>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (root == nullptr) return res;
    
    queue<TreeNode*> q;
    q.push(root);
    bool left_to_right = true; // Spatial boolean toggle
    
    while (!q.empty()) {
        int level_size = q.size(); // Rigid depth boundary
        
        // Dynamically pre-allocate vector space to emulate deque front-insertion geometrically
        vector<int> current_level(level_size);
        
        for (int i = 0; i < level_size; i++) {
            TreeNode* current = q.front();
            q.pop();
            
            // Evaluate strictly determined spatial index projection
            int index = left_to_right ? i : (level_size - 1 - i);
            current_level[index] = current->val; // Geometric assignment
            
            // Temporal queues remain strictly un-inverted to maintain descendant spatial integrity
            if (current->left != nullptr) q.push(current->left);
            if (current->right != nullptr) q.push(current->right);
        }
        
        res.push_back(current_level);
        left_to_right = !left_to_right; // Strict structural inversion
    }
    
    return res;
}
```

> [!important]
> The temporal `std::queue` directing the topological traversal of children *must remain structurally strictly left-to-right* during the loop. The absolute inversion must exclusively impact the output storage indexing, otherwise topological spatial relations between subsequent layers disintegrate geometrically.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Each internal element traverses the FIFO queue and resolves into a directly bounded vector index exclusively once.
- **Space Complexity:** $\mathcal{O}(N)$. Bound strictly by the maximum spatial width of the contiguous binary tree level $\approx \frac{N}{2}$.
