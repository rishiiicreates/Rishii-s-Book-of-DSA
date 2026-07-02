---
type: concept
tags: [binary_tree, bst, cpp, construction]
date: 2026-06-30
---
# Construct Binary Tree from Preorder and Inorder Traversal

## Problem Statement
Given two integer arrays representing the preorder and inorder traversal of a binary tree, construct and return the binary tree.

## Approach / Intuition
The first element in the preorder traversal is always the root. Locate this root in the inorder traversal. Elements to its left in the inorder [[Array]] form the left subtree, and elements to its right form the right subtree. Use a hash map to store inorder indices for $O(1)$ lookups, and recursively construct the left and right subtrees by passing appropriate boundaries.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$ for hash map and recursion stack

## Sample Code
```cpp
#include <vector>
#include <unordered_map>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

TreeNode* buildTreeHelper(vector<int>& preorder, int preStart, int preEnd, 
                          vector<int>& inorder, int inStart, int inEnd, 
                          unordered_map<int, int>& inMap) {
    if (preStart > preEnd || inStart > inEnd) return NULL;
    
    TreeNode* root = new TreeNode(preorder[preStart]);
    int inRoot = inMap[root->val];
    int numsLeft = inRoot - inStart;
    
    root->left = buildTreeHelper(preorder, preStart + 1, preStart + numsLeft, 
                                 inorder, inStart, inRoot - 1, inMap);
    root->right = buildTreeHelper(preorder, preStart + numsLeft + 1, preEnd, 
                                  inorder, inRoot + 1, inEnd, inMap);
                                  
    return root;
}

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    unordered_map<int, int> inMap;
    for (int i = 0; i < inorder.size(); i++) {
        inMap[inorder[i]] = i;
    }
    return buildTreeHelper(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1, inMap);
}
```

## New Keywords / STL Used
`unordered_map`

## Edge Cases
Empty traversal arrays, trees with duplicate values (if not guaranteed distinct).
