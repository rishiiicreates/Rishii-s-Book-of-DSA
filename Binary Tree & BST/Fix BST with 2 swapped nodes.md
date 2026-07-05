# Fix BST with 2 swapped nodes

## Problem Statement
- two nodes of a BST are swapped by mistake. Recover the tree without changing its structure.

## Approach / Intuition
- since an [[Inorder Traversal]] of a BST yields a strictly increasing sequence, we can track the `prev` node during traversal. If `prev->val > curr->val`, we've found a violation. The first time this happens, `prev` is the first swapped node, and `curr` is a candidate for the second. If it happens again, the new `curr` is definitely the second swapped node. After finding both, we simply swap their values.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(H)$ for recursion stack.

## Sample Code
```cpp
#include <algorithm>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
    TreeNode* first = nullptr;
    TreeNode* middle = nullptr;
    TreeNode* last = nullptr;
    TreeNode* prev = nullptr;
    
    void inorder(TreeNode* root) {
        if (!root) return;
        
        inorder(root->left);
        
        if (prev != nullptr && root->val < prev->val) {
            if (first == nullptr) {
                first = prev;
                middle = root;
            } else {
                last = root;
            }
        }
        prev = root;
        
        inorder(root->right);
    }
    
public:
    void recoverTree(TreeNode* root) {
        inorder(root);
        
        if (first && last) {
            std::swap(first->val, last->val);
        } else if (first && middle) {
            std::swap(first->val, middle->val);
        }
    }
};
```

## New Keywords / STL Used
- `std::swap`

## Edge Cases
- swapped nodes are adjacent in inorder traversal
- swapped nodes are far apart
- swapped nodes are parent and child

NEXT: [[Index]]
