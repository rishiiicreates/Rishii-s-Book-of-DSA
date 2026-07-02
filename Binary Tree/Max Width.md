---
type: concept
tags: [binary-tree, cpp, math, bfs, level-order]
date: 2026-07-01
---
# Maximum Width of a Binary Tree

## Problem Statement
Given a Binary Tree $T$, compute the absolute Maximum Topological Width across all geometric levels. The width of a level is defined as the spatial length between the structurally leftmost and rightmost non-null nodes, inclusive of all mathematically implied `null` gaps between them.

---

## Approach: Geometric Indexing (Heap Isomorphism)

Because internal structural gaps contribute to the overall spatial width, a purely structural traversal fails. We must mathematically index the tree isomorphic to a canonical Array-based Binary Heap geometry.
Let a parent node reside at absolute geometric index $i$.
1. Its strictly Left Child mathematically maps to index $2i$.
2. Its strictly Right Child mathematically maps to index $2i + 1$.

We execute a Breadth-First Search (BFS) using a Queue to traverse the topology level-by-level. For any given level, the spatial width is identically the geometric difference between the first (leftmost) mapped index and the last (rightmost) mapped index:
$$ \text{Width} = (\text{Index}_{last} - \text{Index}_{first}) + 1 $$

---

## Code Implementation

```cpp
#include <queue>
#include <algorithm>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int widthOfBinaryTree(TreeNode* root) {
    if (!root) return 0;
    
    int max_width = 0;
    // Queue stores {Node, Absolute Geometric Index}
    queue<pair<TreeNode*, unsigned long long>> q;
    q.push({root, 0});
    
    while (!q.empty()) {
        int level_size = q.size();
        
        // Normalize geometric origin to prevent integer overflow
        unsigned long long level_min = q.front().second;
        unsigned long long first_index = 0, last_index = 0;
        
        for (int i = 0; i < level_size; i++) {
            TreeNode* current = q.front().first;
            // Shift origin to mathematically map to 0
            unsigned long long mapped_index = q.front().second - level_min;
            q.pop();
            
            if (i == 0) first_index = mapped_index;
            if (i == level_size - 1) last_index = mapped_index;
            
            if (current->left) {
                q.push({current->left, mapped_index * 2});
            }
            if (current->right) {
                q.push({current->right, mapped_index * 2 + 1});
            }
        }
        
        max_width = max(max_width, static_cast<int>(last_index - first_index + 1));
    }
    
    return max_width;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ linear topological scan.
- **Space Complexity:** $O(N)$ bounded by the maximum queue cardinality, which spatially peaks at the lowest geometric level ($O(N/2)$ for a dense tree).

> [!warning]
> **Integer Overflow Trap:** If the tree is pathologically degenerate (e.g., $100$ right-leaning nodes), the absolute index evaluation computes $2^{100}$, which mathematically shatters standard $64$-bit integer bounds. Normalizing the spatial origin for each level (`index - level_min`) is absolutely critical to preserve modular safety within standard machine architecture limits.
