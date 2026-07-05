# Vertical Sum

## Problem Statement
- given a binary tree, find the vertical sum of the nodes that are in the same vertical line.

## Approach / Intuition
- we perform a traversal (such as [[DFS]] or [[Level Order Traversal]]) keeping track of the horizontal distance (HD) from the root. The root is at HD 0. Left children have an HD of `parent_HD - 1`, and right children have `parent_HD + 1`. We use a hash map (or ordered map) to accumulate the sum of nodes that fall on the same HD.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \log N)$ if using `std::map` to keep HD sorted, or $O(N)$ if using `std::unordered_map` with min/max tracking.
- **[[space Complexity]]:** $O(N)$ for the map and recursion stack.

## Sample Code
```cpp
#include <map>
#include <vector>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void verticalSumUtil(TreeNode* root, int hd, std::map<int, int>& m) {
    if (!root) return;
    
    verticalSumUtil(root->left, hd - 1, m);
    m[hd] += root->val;
    verticalSumUtil(root->right, hd + 1, m);
}

std::vector<int> verticalSum(TreeNode* root) {
    std::map<int, int> m;
    verticalSumUtil(root, 0, m);
    
    std::vector<int> res;
    for (auto it : m) {
        res.push_back(it.second);
    }
    
    return res;
}
```

## New Keywords / STL Used
- `std::map`, `std::vector`

## Edge Cases
- empty tree
- completely skewed tree (all nodes on different HDs, or zigzagging on same HDs)
- negative node values

NEXT: [[Index]]
