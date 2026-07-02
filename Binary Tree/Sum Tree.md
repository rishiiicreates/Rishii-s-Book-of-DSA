---
type: concept
tags: [binary-tree, recursion, cpp]
date: 2026-06-30
---
# Sum Tree Verification

## Problem Statement
Given a binary tree, mathematically verify if it qualifies as a **Sum Tree**. A Sum Tree is defined as a binary tree where the value of every non-leaf node $N_i$ is strictly equal to the sum of all node values present in its left subtree and right subtree. 

*Note:* Empty trees and single-node trees (leaf nodes) trivially satisfy the conditions of a Sum Tree by mathematical convention.

---

## Approach: Bottom-Up Accumulation (Post-Order Traversal)

A naive approach calculates the sum of the left and right subtrees independently for every node, resulting in $O(N^2)$ time complexity due to redundant topological traversals.

We can optimize this to $O(N)$ by propagating subtree sums upwards in a single [[Post-Order Traversal]]. Each recursive frame must return two pieces of state information:
1. Is the subtree a valid Sum Tree?
2. What is the total sum of the subtree (including the root)?

To optimize state transfer in C++, we return an integer representing the mathematical sum of the subtree. If a subtree is determined to be invalid, we return a sentinel value (e.g., `-1`, assuming all node values are strictly positive) or utilize a paired struct `std::pair<bool, int>`.

1. **Base Cases:** 
   - If node is `nullptr`, it is valid and its sum is $0$.
   - If node is a leaf, it is valid and its sum is its own data.
2. **Recursive Evaluation:**
   - Recursively fetch the sum of the left subtree ($L$). If $L$ is invalid, propagate invalidity.
   - Recursively fetch the sum of the right subtree ($R$). If $R$ is invalid, propagate invalidity.
3. **Verification Step:**
   - For a non-leaf node, check if `node->data == L + R`.
   - If true, return the total topological sum: `node->data + L + R` (or simply `2 * node->data`).
   - If false, return the invalidity sentinel.

---

## Code Implementation

```cpp
#include <utility>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

// Returns {is_valid, subtree_sum}
pair<bool, int> isSumTreeFast(Node* root) {
    // Base Case 1: Empty node is a valid Sum Tree with sum 0
    if (root == nullptr) {
        return {true, 0};
    }
    
    // Base Case 2: Leaf node is a valid Sum Tree with sum equal to its value
    if (root->left == nullptr && root->right == nullptr) {
        return {true, root->data};
    }
    
    // Post-Order: Traverse Left and Right
    pair<bool, int> leftState = isSumTreeFast(root->left);
    pair<bool, int> rightState = isSumTreeFast(root->right);
    
    // Check constraint validity
    bool isLeftValid = leftState.first;
    bool isRightValid = rightState.first;
    bool isCurrentValid = (root->data == leftState.second + rightState.second);
    
    // Combine state
    if (isLeftValid && isRightValid && isCurrentValid) {
        // Return validity and total sum (node + left_sum + right_sum)
        return {true, root->data + leftState.second + rightState.second};
    }
    
    // Invalid state propagation
    return {false, 0};
}

bool isSumTree(Node* root) {
    return isSumTreeFast(root).first;
}
```

> [!warning]
> The reliance on sentinel integer values like `-1` for invalidity fails if the binary tree nodes can legitimately contain negative integers. Using `std::pair<bool, int>` strictly bypasses this edge case, ensuring mathematical rigor regardless of domain values.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. The algorithm accesses each node exactly once during the post-order traversal, performing strictly constant-time arithmetic and boolean logic.
- **Space Complexity:** $O(H)$, where $H$ is the maximum depth of the tree, representing the memory overhead of the recursive call stack.
