# Bottom View of Binary Tree

## Problem Statement
- given a binary tree, return the nodes visible when viewed from the bottom, ordered from left to right.

## Approach / Intuition
- like the top view, assign a horizontal distance (HD) to each node. Perform a [[Breadth-First Search]] (BFS). Use a map to continually update the node stored at each HD. Because BFS traverses level by level, the last node encountered at any HD will naturally be the bottom-most node.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \log N)$ due to map updates
- **[[space Complexity]]:** $O(N)$ for queue and map

## Sample Code
```cpp
#include <vector>
#include <map>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

vector<int> bottomView(TreeNode *root) {
    vector<int> ans;
    if (!root) return ans;
    
    map<int, int> mpp;
    queue<pair<TreeNode*, int>> q;
    q.push({root, 0});
    
    while (!q.empty()) {
        auto it = q.front();
        q.pop();
        TreeNode* node = it.first;
        int line = it.second;
        
        mpp[line] = node->val;
        
        if (node->left) q.push({node->left, line - 1});
        if (node->right) q.push({node->right, line + 1});
    }
    
    for (auto it : mpp) {
        ans.push_back(it.second);
    }
    return ans;
}
```

## New Keywords / STL Used
- `map`, map update

## Edge Cases
- empty tree, overlapping nodes at same HD where deeper node shadows the upper one.

NEXT: [[Index]]
