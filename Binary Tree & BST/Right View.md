# Right View of Binary Tree

## Problem Statement
- given a binary tree, return the values of the nodes you can see when the tree is viewed from the right side.

## Approach / Intuition
- similar to the left view, use [[Depth-First Search]] but traverse the right child before the left child. Maintain the current depth and add the node to the result list if it's the first time reaching this depth. [[Breadth-First Search]] can also be used by taking the last element in the queue at each level.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(H)$ for DFS recursion stack

## Sample Code
```cpp
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

void solve(TreeNode* root, int level, vector<int>& res) {
    if (!root) return;
    if (res.size() == level) res.push_back(root->val);
    solve(root->right, level + 1, res);
    solve(root->left, level + 1, res);
}

vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    solve(root, 0, res);
    return res;
}
```

## New Keywords / STL Used
- dFS traversal

## Edge Cases
- empty tree, single node tree, tree skewed to the left.

NEXT: [[Index]]
