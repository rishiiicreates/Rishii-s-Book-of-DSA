---
type: concept
tags: [binary_tree, bst, cpp, two-pointers, inorder]
date: 2026-06-30
---
# Pair with given sum

## Problem Statement
Given the root of a Binary Search Tree and a target number $k$, return true if there exist two elements in the BST such that their sum is equal to the given target.

## Approach / Intuition
One approach is to do an [[Inorder Traversal]] to store elements in a sorted array, then use [[Two Pointers]] to find the pair. A more space-optimized approach utilizes a [[BST Iterator]] to simulate the two-pointer technique directly on the tree. We create one iterator that yields elements in ascending order (normal inorder) and another that yields elements in descending order (reverse inorder).

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(H)$ using BST Iterator.

## Sample Code
```cpp
#include <stack>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class BSTIterator {
    std::stack<TreeNode*> st;
    bool reverse;

    void pushAll(TreeNode* node) {
        while (node) {
            st.push(node);
            node = reverse ? node->right : node->left;
        }
    }

public:
    BSTIterator(TreeNode* root, bool isReverse) {
        reverse = isReverse;
        pushAll(root);
    }

    int next() {
        TreeNode* tmp = st.top();
        st.pop();
        if (reverse) pushAll(tmp->left);
        else pushAll(tmp->right);
        return tmp->val;
    }
};

bool findTarget(TreeNode* root, int k) {
    if (!root) return false;
    
    BSTIterator l(root, false);
    BSTIterator r(root, true);
    
    int i = l.next();
    int j = r.next();
    
    while (i < j) {
        if (i + j == k) return true;
        if (i + j < k) i = l.next();
        else j = r.next();
    }
    
    return false;
}
```

## New Keywords / STL Used
`std::stack`

## Edge Cases
- Tree has fewer than two nodes
- Multiple pairs sum to target
- Target is twice the value of one node (must avoid pairing node with itself)
