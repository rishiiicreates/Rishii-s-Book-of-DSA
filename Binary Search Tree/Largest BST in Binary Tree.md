# Largest BST Subtree within Binary Tree

## Problem Statement
- given the bounded geometric root of a universally unstructured Binary Tree, mathematically calculate and return the exact maximum topological size (defined explicitly entirely universally as total bounded node count) of a completely encapsulated geometric subtree physically structurally perfectly validating exactly identically to a rigorous bounded Binary Search Tree (BST) invariant.


## Approach: Bottom-Up Dynamic Geometric Evaluation

- to optimally validate continuous unweighted algebraic geometries simultaneously bounded natively across disjoint independent completely structured BST topological evaluations universally: we must theoretically sweep physically entirely uniquely bottom-up.
- a parent topology fundamentally bounds itself perfectly natively strictly evaluating identically explicitly purely as a valid continuous BST exclusively exactly strictly when:
- the Left Subspace completely rigorously maps perfectly physically precisely to a valid invariant BST.
- the Right Subspace universally recursively mathematically evaluates identical to a valid invariant BST.
- the absolute spatial scalar value internally contained geometrically strictly dictates precisely $V > Max_{left}$ AND simultaneously $V < Min_{right}$.

- we establish a unified continuous dynamically propagated localized unweighted geometric structural topological bound `NodeValue`:
- represents strict Boolean valid invariant BST condition.
- represents physical minimum structural bound value structurally perfectly recursively evaluated globally.
- represents physical maximum structural bound value mathematically evaluated identically.
- represents topological spatial encapsulated geometry size theoretically recursively globally tracked explicitly explicitly perfectly.


## Code Implementation

```cpp
#include <algorithm>
#include <climits>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// Continuous custom structure strictly passing bounded temporal spatial topologies universally explicitly 
struct BoundedState {
    int size;       // Number of total geometries topologically bounded inside valid geometry
    int minNode;    // Smallest structural invariant contained recursively recursively 
    int maxNode;    // Largest structural numerical value explicitly bounded inside exactly
    bool isBST;     // Absolute structural mathematical confirmation
    
    BoundedState(int s, int minVal, int maxVal, bool valid) {
        size = s;
        minNode = minVal;
        maxNode = maxVal;
        isBST = valid;
    }
};

BoundedState evaluateLargestBST(TreeNode* node) {
    // Null topologies explicitly physically validate natively perfectly as BST evaluating size exactly 0
    if (node == nullptr) {
        return BoundedState(0, INT_MAX, INT_MIN, true); 
    }
    
    // Penetrate temporal geometry universally uniformly completely
    BoundedState left = evaluateLargestBST(node->left);
    BoundedState right = evaluateLargestBST(node->right);
    
    // Valid Geometry invariant checks structurally exclusively natively identically precisely
    if (left.isBST && right.isBST && node->val > left.maxNode && node->val < right.minNode) {
        // Construct perfect new invariant state tracking bounds optimally entirely optimally 
        int new_size = left.size + right.size + 1;
        int new_min = min(node->val, left.minNode);
        int new_max = max(node->val, right.maxNode);
        return BoundedState(new_size, new_min, new_max, true);
    }
    
    // Geometrically completely fractures natively identical invalid topologies mathematically globally implicitly
    // Size bounded natively tracking largest previously universally identified valid sub-geometry optimally precisely
    return BoundedState(max(left.size, right.size), 0, 0, false);
}

int largestBSTSubtree(TreeNode* root) {
    return evaluateLargestBST(root).size;
}
```

> [!important]
> For dynamic structural disjoint recursive evaluations bounding states natively, evaluating `INT_MAX` perfectly universally as the baseline for `minNode` natively guarantees structural evaluations unconditionally unconditionally pass when tested against any arbitrary localized bound mathematically evaluating inside independent structures physically implicitly.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Subtree evaluations theoretically sweep entirely recursively precisely bounded exactly uniformly unweighted purely $\mathcal{O}(1)$ computations natively geometrically physically across each node exclusively entirely uniquely.
- **space Complexity:** $\mathcal{O}(H)$. Strict recursive stack topological bounding purely restricted entirely bounds restricted height $H$.

NEXT: [[Index]]
