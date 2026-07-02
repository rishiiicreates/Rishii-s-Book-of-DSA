---
type: concept
tags: [binary-tree, construction, hashing, cpp]
date: 2026-06-30
---
# Tree from Preorder and Inorder Traversal

## Problem Statement
Given two arrays representing the **Preorder** and **Inorder** traversals of a binary tree, mathematically reconstruct the original binary tree. Assume all values in the tree are strictly unique.

*Example:* 
`preorder` = $[3, 9, 20, 15, 7]$
`inorder`  = $[9, 3, 15, 20, 7]$
*Result:* Root node `3`, with left child `9` and right child `20`.

---

## Approach: Divide and Conquer (Hash-Mapped Subarrays)

Reconstructing a tree requires two fundamental components:
1. **Root Identification:** The root of any given subtree is universally located at the absolute front (index $0$) of its corresponding `preorder` traversal slice.
2. **Topology Identification:** By locating this root value within the `inorder` traversal, we mathematically partition the `inorder` array into two mutually exclusive sets:
   - Elements strictly to the left of the root constitute the **Left Subtree**.
   - Elements strictly to the right of the root constitute the **Right Subtree**.

This establishes a recursive invariant. To prevent an $O(N^2)$ degradation caused by linear scanning to find the root in the `inorder` array, we preprocess the `inorder` array into a [[Hash Map]], achieving $O(1)$ lookups.

1. **Map Construction:** Map every value in `inorder` to its topological index.
2. **Recursive Transformation:** `buildTree(preStart, preEnd, inStart, inEnd)`
   - Extract the root value `rootVal = preorder[preStart]`.
   - Find the pivot index `inRoot = map[rootVal]`.
   - Calculate the bounded length of the left subtree: `leftNodes = inRoot - inStart`.
   - Recursively construct `node->left` mapping to:
     - Preorder bounds: `[preStart + 1, preStart + leftNodes]`
     - Inorder bounds: `[inStart, inRoot - 1]`
   - Recursively construct `node->right` mapping to:
     - Preorder bounds: `[preStart + leftNodes + 1, preEnd]`
     - Inorder bounds: `[inRoot + 1, inEnd]`
3. **Base Case:** The recursion terminates mathematically when `preStart > preEnd` or `inStart > inEnd`, returning `nullptr`.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

Node* buildTreeHelper(const vector<int>& preorder, int preStart, int preEnd,
                      const vector<int>& inorder, int inStart, int inEnd,
                      unordered_map<int, int>& inMap) {
    // Base constraint violation
    if (preStart > preEnd || inStart > inEnd) {
        return nullptr;
    }
    
    // Step 1: Root instantiation
    Node* root = new Node(preorder[preStart]);
    
    // Step 2: Inorder partition index
    int inRoot = inMap[root->data];
    int leftNodes = inRoot - inStart;
    
    // Step 3: Divide and Conquer
    root->left = buildTreeHelper(preorder, preStart + 1, preStart + leftNodes,
                                 inorder, inStart, inRoot - 1, inMap);
                                 
    root->right = buildTreeHelper(preorder, preStart + leftNodes + 1, preEnd,
                                  inorder, inRoot + 1, inEnd, inMap);
                                  
    return root;
}

Node* buildTree(vector<int>& preorder, vector<int>& inorder) {
    unordered_map<int, int> inMap;
    // Precompute spatial indices for O(1) retrieval
    for (int i = 0; i < inorder.size(); ++i) {
        inMap[inorder[i]] = i;
    }
    
    return buildTreeHelper(preorder, 0, preorder.size() - 1,
                           inorder, 0, inorder.size() - 1, inMap);
}
```

> [!important]
> Mathematical reconstruction is absolutely impossible if duplicate values exist in the tree. Duplicate values would cause collisions in the `inMap`, rendering the separation of the left and right subtrees topologically ambiguous.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. Building the hash map evaluates in $O(N)$ expected time. The recursive function processes each node precisely once, taking $O(1)$ arithmetic operations per node.
- **Space Complexity:** $O(N)$. The hash map absorbs $O(N)$ dynamic memory. The recursive call stack utilizes $O(H)$ memory, bound maximally by $O(N)$. Total space is strictly $O(N)$.
