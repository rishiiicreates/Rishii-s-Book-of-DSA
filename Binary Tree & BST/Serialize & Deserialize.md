# Serialize and Deserialize Binary Tree

## Problem Statement
- design an algorithm to serialize a binary tree to a string and deserialize that string back to the original binary tree.

## Approach / Intuition
- serialization: Use [[Breadth-First Search]] (Level Order Traversal) to convert the tree into a [[String]] using a queue. Record null nodes with a special character like '#'.
- deserialization: Use a stringstream to parse the string. Rebuild the tree level by level using a queue, creating child nodes from the parsed string values and linking them to dequeued parent nodes.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <string>
#include <queue>
#include <sstream>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Codec {
public:
    string serialize(TreeNode* root) {
        if (!root) return "";
        string s = "";
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* curNode = q.front();
            q.pop();
            if (curNode == NULL) s.append("#,");
            else {
                s.append(to_string(curNode->val) + ",");
                q.push(curNode->left);
                q.push(curNode->right);
            }
        }
        return s;
    }

    TreeNode* deserialize(string data) {
        if (data.size() == 0) return NULL;
        stringstream s(data);
        string str;
        getline(s, str, ',');
        TreeNode* root = new TreeNode(stoi(str));
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            
            getline(s, str, ',');
            if (str == "#") node->left = NULL;
            else {
                node->left = new TreeNode(stoi(str));
                q.push(node->left);
            }
            
            getline(s, str, ',');
            if (str == "#") node->right = NULL;
            else {
                node->right = new TreeNode(stoi(str));
                q.push(node->right);
            }
        }
        return root;
    }
};
```

## New Keywords / STL Used
- `stringstream`, `getline`, `stoi`, `to_string`

## Edge Cases
- empty tree, heavily unbalanced tree.

NEXT: [[Index]]
