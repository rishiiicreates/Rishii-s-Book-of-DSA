# Serialize and Deserialize Binary Tree

## Problem Statement
- mathematically define and implement an algorithm to transform a complex hierarchical 2D graph (Binary Tree) into a strictly continuous 1D scalar string (Serialization), and reconstruct the exact absolute original 2D geometric structure from that string (Deserialization).


## Approach: Level Order (BFS) Topological Mapping

- while DFS (Preorder with explicit null markers) works optimally, Breadth-First Search (BFS) mirrors the standard geometric layer-by-layer topology intuitively (similar to LeetCode's canonical array representations).

- **serialization:**
   - execute a theoretical [[Level Order]] traversal (BFS) using a FIFO `std::queue`.
   - if a node structurally exists, sequentially map its integer scalar to the string buffer followed by a structural delimiter (e.g., `,`).
   - if a node is an absolute null pointer, insert a distinct geometric tombstone marker (e.g., `#`) to strictly define terminal bounds.
- **deserialization:**
   - tokenize the serial continuous string strictly on the delimiter `,`.
   - the primary initial token theoretically represents the absolute root.
   - employ a FIFO queue tracking structural parents.
   - for every extracted parent from the queue, consume the succeeding two tokens as its left and right geometric children. Re-link them spatially and immediately enqueue any structurally existent children.


## Code Implementation

```cpp
#include <string>
#include <queue>
#include <sstream>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Codec {
public:
    // Transform 2D geometry into 1D continuous string
    string serialize(TreeNode* root) {
        if (!root) return "";
        string s = "";
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode* current = q.front();
            q.pop();
            
            if (current == nullptr) {
                s += "#,"; // Structural tombstone boundary
            } else {
                s += to_string(current->val) + ",";
                q.push(current->left);
                q.push(current->right);
            }
        }
        return s;
    }

    // Reconstruct 2D geometry from 1D string state
    TreeNode* deserialize(string data) {
        if (data.empty()) return nullptr;
        
        stringstream s(data);
        string str;
        getline(s, str, ','); // Extract absolute root token
        
        TreeNode* root = new TreeNode(stoi(str));
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode* parent = q.front();
            q.pop();
            
            // Re-link Left geometric bound
            getline(s, str, ',');
            if (str != "#") {
                parent->left = new TreeNode(stoi(str));
                q.push(parent->left);
            }
            
            // Re-link Right geometric bound
            getline(s, str, ',');
            if (str != "#") {
                parent->right = new TreeNode(stoi(str));
                q.push(parent->right);
            }
        }
        
        return root;
    }
};
```

> [!important]
> Stringstream (`std::stringstream`) and `getline` provide highly rigorous $O(N)$ string extraction mechanisms, bypassing excessive spatial memory allocations common to standard string splitting algorithms.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$ for both structural transformations. Each geometric node and terminal bound is evaluated and tokenized exactly once.
- **space Complexity:** $\mathcal{O}(N)$ bounded space. The queue maintains at most $\approx \frac{N}{2}$ entities, and the continuous serialized string mathematically demands $\mathcal{O}(N)$ spatial allocation.

NEXT: [[Index]]
