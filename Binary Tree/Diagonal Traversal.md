# Diagonal Traversal of Binary Tree

## Problem Statement
- given a discrete binary tree, mathematically partition its absolute geometric structure into a sequence of purely diagonal components. Elements situated on the identical principal topological diagonal must be grouped iteratively, maintaining strict descending depth causality (i.e., top-down within each specific discrete diagonal).


## Approach: Topological Depth Masking (DFS/BFS)

- the geometric definition of a rightward descending diagonal defines an invariant structural slope: any given node $U$, and its immediate right child $U_R$, mathematically exist upon the absolute identical diagonal space. A transition strictly to a left child $U_L$ structurally shifts the topological coordinate vector exactly one discrete spatial diagonal to the "left".

- we establish a localized tracking coordinate $D$ representing the diagonal scalar offset:
- root evaluates strictly to $D = 0$.
- for right topological shifts: $D_{right} = D$.
- for left topological shifts: $D_{left} = D + 1$.

- algorithm (FIFO Loop / Queue):
- execute a continuous loop to trace discrete unbranched rightward structures.
- initialize a LIFO/FIFO boundary (typically a queue) to defer evaluations of disjoint leftward shifted subspaces.
- for every node popped, iteratively penetrate strictly down its continuous sequence of right spatial children.
- during this continuous rightward descent, push the evaluated spatial node into the current scalar diagonal group.
- whenever a left child structurally exists, enqueue it. Because the queue processes spatially, all elements defined by exactly $D=k$ are processed fully before $D=k+1$.


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

vector<vector<int>> diagonalTraversal(TreeNode* root) {
    vector<vector<int>> res;
    if (root == nullptr) return res;
    
    // Spatial queue bounding delayed geometrical subspaces
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int level_size = q.size();
        vector<int> current_diagonal;
        
        // Evaluate all continuous right-ward structures bounding current spatial depth D
        for (int i = 0; i < level_size; i++) {
            TreeNode* current = q.front();
            q.pop();
            
            // Unidirectional geometry loop (strict Rightward Penetration)
            while (current != nullptr) {
                current_diagonal.push_back(current->val); // Evaluate temporal invariant
                
                // Left children strictly bounded to subsequent D+1 depth
                if (current->left != nullptr) {
                    q.push(current->left);
                }
                
                // Continuous transition along the identical structural slope
                current = current->right;
            }
        }
        
        res.push_back(current_diagonal);
    }
    
    return res;
}
```

> [!tip]
> A recursive DFS approach tracking the $D$ offset as a scalar state parameter mapping directly to a `unordered_map<int, vector<int>>` equivalently yields identical geometric groupings, though it introduces redundant map insertions dominating optimal structural vector appends.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Every geometric node physically traverses the rightward loop and/or the delayed BFS queue strictly at most once.
- **space Complexity:** $\mathcal{O}(N)$. The queue temporally bounds the maximum structural width defined strictly by the density of left-branching geometry.

NEXT: [[Index]]
