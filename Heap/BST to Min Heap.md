---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# BST to Min Heap

## Problem Statement
Convert a given [[Binary Search Tree]] (BST) into a Min-[[Heap]] such that all the values in the left subtree of a node should be less than all the values in the right subtree of the node. The structure of the tree must remain the same.

## Approach / Intuition
A [[Binary Search Tree]]'s inorder traversal yields elements in sorted (ascending) order. To satisfy the given condition (left child < right child) and the min-heap property (parent < children), we must perform a preorder traversal to populate the tree with the sorted elements. 
1. Perform inorder traversal of the BST to store elements in a sorted array.
2. Perform preorder traversal of the tree and replace the node values sequentially with elements from the sorted array.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of nodes.
- **[[Space Complexity]]:** O(N) to store the inorder traversal array.

## Sample Code
```cpp
#include <vector>

using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

void inorderTraversal(Node* root, vector<int>& arr) {
    if (root == nullptr) return;
    inorderTraversal(root->left, arr);
    arr.push_back(root->data);
    inorderTraversal(root->right, arr);
}

void preorderFill(Node* root, vector<int>& arr, int& index) {
    if (root == nullptr) return;
    root->data = arr[index++];
    preorderFill(root->left, arr, index);
    preorderFill(root->right, arr, index);
}

void convertToMinHeap(Node* root) {
    vector<int> arr;
    inorderTraversal(root, arr);
    int index = 0;
    preorderFill(root, arr, index);
}
```

## New Keywords / STL Used
None

## Edge Cases
- Empty tree.
- Tree with a single node.
