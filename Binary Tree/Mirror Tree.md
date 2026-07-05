# Mirror Tree (Invert Binary Tree)

## Problem Statement
- given the root of a binary tree, mathematically invert the tree such that for every node $N_i$ in the tree, its left child and right child are swapped. The resulting tree is the topological mirror image of the original tree.


## Approach: Post-Order Traversal (Recursion)

- the operation of mirroring a tree exhibits strictly optimal substructure. A tree rooted at node $U$ is perfectly mirrored if and only if:
- the left subtree of $U$ is perfectly mirrored.
- the right subtree of $U$ is perfectly mirrored.
- the spatial pointers `U->left` and `U->right` are subsequently swapped.

- this bottom-up resolution maps elegantly to a [[Post-Order Traversal]].

- **base Case:** If the current node is `nullptr`, the mirror of an empty tree is an empty tree. Return `nullptr`.
- **recursive Step:** Recursively mirror the left subtree and the right subtree.
- **transformation:** Swap the pointers to the left and right children of the current node using `std::swap`.
- **return State:** Return the current node, whose subtrees are now fully inverted.

> [!tip]
> While a Pre-Order traversal (swap first, then recurse) is also mathematically valid, the Post-Order traversal is often preferred as it guarantees that child topologies are fully resolved before altering the parent's structure.


## Code Implementation

```cpp
#include <algorithm>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

void mirrorTree(Node* node) {
    // Base Case: Empty subtree
    if (node == nullptr) {
        return;
    }
    
    // Post-Order Step 1: Recursively mirror the left subtree
    mirrorTree(node->left);
    
    // Post-Order Step 2: Recursively mirror the right subtree
    mirrorTree(node->right);
    
    // Post-Order Step 3: Swap the topological pointers
    swap(node->left, node->right);
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$. The algorithm visits every node exactly once to perform a constant-time pointer swap operation. Therefore, the total processing time is strictly linear.
- **space Complexity:** $O(H)$, where $H$ is the height of the tree, stemming from the implicit recursion call stack. For a perfectly balanced tree, this is $O(\log N)$. For a skewed pathological tree, this degrades to $O(N)$.

NEXT: [[Index]]
