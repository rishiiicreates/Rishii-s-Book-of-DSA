# Leaf at Same Level

## Problem Statement
- given a binary tree, mathematically verify if all of its leaf nodes reside at the strictly identical depth level.

- *definition:* A leaf node is defined as an entity with zero children. Depth level $D$ is defined relative to the root node (typically starting at $D = 1$ or $D = 0$).


## Approach: Depth-First Search with Global State Validation

- the most efficient algorithm maintains a global (or reference-passed) invariant that records the depth level of the **very first leaf node encountered** during a standard [[Depth First Search]]. All subsequently discovered leaf nodes are mathematically required to perfectly match this recorded depth.

- **state Invariants:**
   - `first_leaf_level`: Initialized to an invalid sentinel value (e.g., $0$). When the first leaf is reached, this value is permanently locked to that depth.
   - `current_level`: The active depth metric tracking the current node's distance from the root.
- **null Base Case:** If the current node is `nullptr`, it does not violate any constraints. Return `true`.
- **leaf Validation:** If `node->left == nullptr` and `node->right == nullptr`:
   - if `first_leaf_level == 0` (this is the absolute first leaf evaluated), assign `first_leaf_level = current_level` and return `true`.
   - if `first_leaf_level != 0`, return the boolean expression `current_level == first_leaf_level`.
- **recursive Propagation:** If the current node is internal, recursively validate both the left subtree and the right subtree. If either subtree mathematically violates the level constraint, propagate `false` upwards via a logical AND (`&&`) constraint.


## Code Implementation

```cpp
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

bool checkUtil(Node* root, int current_level, int& first_leaf_level) {
    if (root == nullptr) {
        return true;
    }
    
    // Verify mathematical leaf condition
    if (root->left == nullptr && root->right == nullptr) {
        // First leaf encountered locks the topological level constraint
        if (first_leaf_level == 0) {
            first_leaf_level = current_level;
            return true;
        }
        
        // Strict equality check against the locked constraint
        return (current_level == first_leaf_level);
    }
    
    // Recursive validation (DFS)
    return checkUtil(root->left, current_level + 1, first_leaf_level) &&
           checkUtil(root->right, current_level + 1, first_leaf_level);
}

bool check(Node* root) {
    int first_leaf_level = 0;
    return checkUtil(root, 1, first_leaf_level);
}
```

> [!tip]
> A [[Breadth First Search]] (Level Order Traversal) is technically valid but introduces unnecessary dynamic queue overhead $O(N)$. The DFS recursive stack utilizes only $O(H)$ memory, achieving superior spatial efficiency for deep, structurally sound trees.


## Complexity Analysis
- **time Complexity:** $O(N)$. The algorithm visits every node in the worst-case scenario (e.g., all leaves are on the exact same level, requiring exhaustive mathematical verification).
- **space Complexity:** $O(H)$, where $H$ is the topological height of the tree. The implicit recursion stack overhead is bound by $O(N)$ for skewed pathological structures, but normally sits at $O(\log N)$ for balanced structures.

NEXT: [[Index]]
