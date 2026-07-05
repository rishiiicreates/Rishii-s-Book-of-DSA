# Merge two BSTs

## Problem Statement
- merge two given [[Binary Search Tree]]s into a single sorted list or a balanced BST.

## Approach / Intuition
- perform an [[Inorder Traversal]] on both trees to extract their elements into two sorted arrays. Then, use the [[Merge Sort]] logic to combine the two sorted arrays into one. Finally, build a balanced BST from the merged sorted array, or simply return the sorted elements as required.

## Time & Space Complexity
- **[[time Complexity]]:** O(M + N) where M and N are the number of nodes in the two trees.
- **[[space Complexity]]:** O(M + N) to store the inorder traversals and the merged array.

## Sample Code
```cpp
class Solution {
    void inorder(TreeNode* root, vector<int>& arr) {
        if (!root) return;
        inorder(root->left, arr);
        arr.push_back(root->val);
        inorder(root->right, arr);
    }

public:
    vector<int> mergeBST(TreeNode* root1, TreeNode* root2) {
        vector<int> arr1, arr2, merged;
        inorder(root1, arr1);
        inorder(root2, arr2);
        
        int i = 0, j = 0;
        while (i < arr1.size() && j < arr2.size()) {
            if (arr1[i] <= arr2[j]) {
                merged.push_back(arr1[i++]);
            } else {
                merged.push_back(arr2[j++]);
            }
        }
        
        while (i < arr1.size()) merged.push_back(arr1[i++]);
        while (j < arr2.size()) merged.push_back(arr2[j++]);
        
        return merged;
    }
};
```

## New Keywords / STL Used
- `std::vector`
- `push_back`

## Edge Cases
- one or both trees are empty.
- all elements of one tree are smaller or larger than the other.
- trees have duplicate elements.

NEXT: [[Index]]
