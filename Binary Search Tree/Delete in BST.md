---
type: concept
tags: [binary-search-tree, recursion, deletion, cpp]
date: 2026-06-30
---
# Delete Node in a BST

## Problem Statement
Given the root of a [[Binary Search Tree]] and a scalar value `key`, mathematically locate the node with `val == key`, safely excise it from the topological structure, and ensure the BST properties are flawlessly preserved.

---

## Approach: Hibbard Deletion Algorithm

Deleting a node is the most mathematically intricate operation in a BST, as excising an internal node creates a structural vacuum. We must evaluate three distinct topological cases:

1. **Case 1: Target is a Leaf Node**
   - The node has zero children.
   - *Resolution:* Simply delete the node and return `nullptr` to its parent.

2. **Case 2: Target has Exactly One Child**
   - The node has either an isolated left subtree or an isolated right subtree.
   - *Resolution:* Delete the target node and return its single child to the parent, effectively bypassing the excised node.

3. **Case 3: Target has Two Children (The Critical Case)**
   - Excision creates two orphaned subtrees. We must replace the target node's mathematical value with a value that logically separates the left partition from the right partition.
   - *Resolution:* 
     1. Find the **In-Order Successor** (the absolute minimum value in the right subtree). Alternatively, the In-Order Predecessor (max in left subtree) is equally valid.
     2. Overwrite the target node's `val` with the successor's `val`.
     3. Recursively delete the successor node from the right subtree. The successor is guaranteed to fall into Case 1 or Case 2, as it inherently lacks a left child.

---

## Code Implementation

```cpp
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// Helper function to extract the topological minimum
TreeNode* findMin(TreeNode* node) {
    while (node->left != nullptr) {
        node = node->left;
    }
    return node;
}

TreeNode* deleteNode(TreeNode* root, int key) {
    if (root == nullptr) return root;

    // Phase 1: Locate the target node
    if (key < root->val) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->val) {
        root->right = deleteNode(root->right, key);
    } 
    // Phase 2: Target isolated
    else {
        // Case 1 & 2: Zero or One Child
        if (root->left == nullptr) {
            TreeNode* temp = root->right;
            delete root;
            return temp;
        } else if (root->right == nullptr) {
            TreeNode* temp = root->left;
            delete root;
            return temp;
        }
        
        // Case 3: Two Children
        // Identify mathematical successor
        TreeNode* successor = findMin(root->right);
        
        // Inherit successor's state
        root->val = successor->val;
        
        // Recursively excise the successor node
        root->right = deleteNode(root->right, successor->val);
    }
    
    return root;
}
```

> [!warning]
> Forgetting to `delete root` results in severe memory leaks. Furthermore, when implementing Case 3, never attempt to structurally move the successor node itself (re-wiring left and right pointers). Simply transferring the scalar `val` and recursing is significantly safer and cleaner.

---

## Complexity Analysis
- **Time Complexity:** $O(H)$, where $H$ is the topological height of the tree. Finding the node takes $O(H)$, and traversing to find the successor (if Case 3 triggers) takes at most $O(H)$. Total time remains bounded by tree depth.
- **Space Complexity:** $O(H)$ auxiliary space corresponding to the maximum depth of the recursive call stack.
