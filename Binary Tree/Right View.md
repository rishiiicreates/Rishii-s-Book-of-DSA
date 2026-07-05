# Right View of Binary Tree

## Problem Statement
- given a Binary Tree, mathematically compute its **Right View**. The right view of a binary tree is defined as the ordered set of nodes visible when the tree is observed strictly from its right side. Equivalently, it is the absolute last node encountered at each discrete depth level.


## Approach: Depth-First Search (DFS) with Level Tracking

- this problem is the exact topological mirror of the [[Left View]]. We solve it efficiently by employing a modified [[Pre-Order Traversal]] (DFS) where we prioritize traversing the right child before the left child.

- **state Invariant:** Maintain `max_level_reached` initialized to $0$. Pass `current_level` (1-indexed) into the recursive function.
- **right-First Traversal:** By deliberately exploring `node->right` before `node->left`, we mathematically guarantee that the first time the DFS discovers a specific depth level $L$, it is unconditionally visiting the rightmost node of that level.
- **observation Trigger:** Upon reaching `current_level`, check if `current_level > max_level_reached`.
   - if true, record the node's value as it is the rightmost visible entity. Update `max_level_reached = current_level`.
- **recursion:**
   - traverse `node->right` with `current_level + 1`.
   - traverse `node->left` with `current_level + 1`.


## Code Implementation

```cpp
#include <vector>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

void rightViewUtil(Node* root, int current_level, int& max_level_reached, vector<int>& res) {
    if (root == nullptr) return;
    
    // First time reaching this depth level (right-first guarantee)
    if (current_level > max_level_reached) {
        res.push_back(root->data);
        max_level_reached = current_level;
    }
    
    // Strictly Right-First Traversal
    rightViewUtil(root->right, current_level + 1, max_level_reached, res);
    rightViewUtil(root->left, current_level + 1, max_level_reached, res);
}

vector<int> rightView(Node* root) {
    vector<int> res;
    int max_level_reached = 0;
    
    // Start DFS at depth level 1
    rightViewUtil(root, 1, max_level_reached, res);
    
    return res;
}
```

> [!tip]
> A standard BFS (Level Order Traversal) can also solve this iteratively by overwriting a `level_value` scalar at each level and pushing that value to the result array once the queue exhausts that specific level's size constraint. DFS is primarily preferred for minimizing code footprint.


## Complexity Analysis
- **time Complexity:** $O(N)$. The DFS traverses every node exactly once to verify the depth condition.
- **space Complexity:** $O(H)$, where $H$ is the tree's height, allocating auxiliary space to the recursion call stack. For completely skewed trees, this bound approaches $O(N)$.

NEXT: [[Index]]
