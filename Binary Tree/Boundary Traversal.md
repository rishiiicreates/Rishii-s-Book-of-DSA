# Boundary Traversal of Binary Tree

## Problem Statement
- given a Binary Tree, mathematically compute its **Boundary Traversal**. The boundary traversal is defined topologically as the anti-clockwise contiguous sequence of:
- the root node.
- the left boundary (excluding the root and leaf nodes).
- all leaf nodes (from left to right).
- the right boundary (excluding the root and leaf nodes), traversed strictly in reverse (bottom-up).


## Approach: Topological Decomposition

- the most robust mathematical approach decomposes the problem into three mutually exclusive topological sub-problems, solving each sequentially.

- **left Boundary Phase (Top-Down):**
   - start from `root->left`.
   - if the current node is not a leaf, record its value.
   - mathematically prioritize the left topological path. If `node->left` exists, recurse left. If it does not exist, you are forced to recurse right to maintain the boundary edge.
   - terminate upon reaching a leaf.
- **leaf Nodes Phase (Left-to-Right):**
   - execute a standard [[In-Order Traversal]] (or strictly Left-Right Pre-Order).
   - if a node is a leaf (`node->left == nullptr` and `node->right == nullptr`), record its value.
   - this cleanly captures all leaf nodes universally, regardless of their parent's boundary status.
- **right Boundary Phase (Bottom-Up):**
   - start from `root->right`.
   - mathematically prioritize the right topological path. If `node->right` exists, recurse right. If not, recurse left.
   - **crucial Invariant:** To achieve bottom-up ordering, recursively call the function *before* recording the node's value (post-order operation relative to the sequence).
   - terminate upon reaching a leaf.


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

bool isLeaf(Node* node) {
    return (node->left == nullptr && node->right == nullptr);
}

void collectLeftBoundary(Node* root, vector<int>& res) {
    Node* curr = root->left;
    while (curr) {
        if (!isLeaf(curr)) res.push_back(curr->data);
        if (curr->left) curr = curr->left;
        else curr = curr->right;
    }
}

void collectLeaves(Node* root, vector<int>& res) {
    if (root == nullptr) return;
    if (isLeaf(root)) {
        res.push_back(root->data);
        return;
    }
    collectLeaves(root->left, res);
    collectLeaves(root->right, res);
}

void collectRightBoundary(Node* root, vector<int>& res) {
    Node* curr = root->right;
    vector<int> temp; // Used to reverse the right boundary
    while (curr) {
        if (!isLeaf(curr)) temp.push_back(curr->data);
        if (curr->right) curr = curr->right;
        else curr = curr->left;
    }
    for (int i = temp.size() - 1; i >= 0; --i) {
        res.push_back(temp[i]);
    }
}

vector<int> boundaryTraversal(Node* root) {
    vector<int> res;
    if (root == nullptr) return res;
    
    // 1. Root
    if (!isLeaf(root)) res.push_back(root->data);
    
    // 2. Left Boundary
    collectLeftBoundary(root, res);
    
    // 3. Leaves
    collectLeaves(root, res);
    
    // 4. Right Boundary
    collectRightBoundary(root, res);
    
    return res;
}
```

> [!important]
> The root node must be handled explicitly. If the tree contains only a single node (the root, which is technically also a leaf), handling the root blindly in the left boundary logic will cause duplications. We strictly verify `!isLeaf(root)` before manually injecting the root.


## Complexity Analysis
- **time Complexity:** $O(N)$. The algorithm traverses the left boundary $O(H)$, the right boundary $O(H)$, and iterates over all leaves and internal nodes during the leaf collection phase $O(N)$. Mathematically, no node is visited more than twice, bounding execution to $O(N)$.
- **space Complexity:** $O(H)$ auxiliary space for the recursive call stack during leaf collection, where $H$ is the tree's depth.

NEXT: [[Index]]
