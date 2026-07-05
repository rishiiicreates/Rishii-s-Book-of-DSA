# Floor

## Problem Statement
- given a BST and an integer key, find the floor value of the key in the BST. The floor value is the greatest node value smaller than or equal to the key.

## Approach / Intuition
- we traverse the [[Binary Search Tree]]. If the current node's value equals the key, we found the exact floor. If the current node's value is greater than the key, the floor must be in the left subtree. If the current node's value is less than the key, this node is a potential candidate for the floor. We update our `floor` variable and then move to the right subtree to try and find a larger value that is still $\leq$ the key.

## Time & Space Complexity
- **[[time Complexity]]:** $O(H)$ where $H$ is the tree height.
- **[[space Complexity]]:** $O(1)$

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int floorBST(TreeNode* root, int key) {
    int floor = -1;
    while (root) {
        if (root->val == key) {
            return root->val;
        }
        
        if (key > root->val) {
            floor = root->val;
            root = root->right;
        } else {
            root = root->left;
        }
    }
    return floor;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- key is smaller than all nodes in the BST
- key is exactly present in the BST
- bST is empty

NEXT: [[Index]]
