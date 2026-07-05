# Binary Search Tree (BST)

## Mathematical Definition
- a **Binary Search Tree (BST)** is a node-based binary tree data structure which possesses the following strict topological properties:
- **left Subtree Constraint:** The left subtree of a node contains only nodes with keys strictly less than the node's key.
- **right Subtree Constraint:** The right subtree of a node contains only nodes with keys strictly greater than the node's key.
- **recursive Invariant:** Both the left and right subtrees must themselves unconditionally satisfy the constraints of a Binary Search Tree.

- mathematically, for any given node $N$, the invariant is:
$$ \forall L \in \text{LeftSubtree}(N), \text{key}(L) < \text{key}(N) $$
$$ \forall R \in \text{RightSubtree}(N), \text{key}(R) > \text{key}(N) $$

- *(note: If duplicate keys are permitted, the constraints relax to $\le$ and $\ge$, but classical BSTs assume unique keys unless specified otherwise.)*


## Fundamental Operations and Complexities

- the BST topology mathematically guarantees that the search space is partitioned at every node. Consequently, operations emulate binary search on an array.

- | operation | Average Case | Worst Case |
- | :--- | :--- | :--- |
- | **search** | $O(\log N)$ | $O(N)$ |
- | **insert** | $O(\log N)$ | $O(N)$ |
- | **delete** | $O(\log N)$ | $O(N)$ |
- | **access** | $O(\log N)$ | $O(N)$ |

> [!warning]
> The theoretical $O(\log N)$ bound relies entirely on the assumption that the tree is reasonably balanced. In pathological cases (e.g., sequentially inserting $1, 2, 3, 4, 5$), the BST degenerates into a Linked List, eroding all structural benefits and degrading into $O(N)$ operations. Advanced data structures like **AVL Trees** and **Red-Black Trees** exist solely to dynamically enforce logarithmic height bounds.


## Topological Implications

- **in-Order Traversal:** Executing an [[Inorder Traversal]] (Left, Root, Right) on a valid BST strictly yields the keys in monotonically ascending order. This mathematical guarantee is foundational to solving nearly all BST validation and range-query algorithms.
- **successors and Predecessors:** The logical successor (next largest value) of any node $N$ is unconditionally the minimum value node in $N$'s right subtree. Conversely, the logical predecessor is the maximum value node in $N$'s left subtree.


## Standard Node Implementation

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    
    // Constructor
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

NEXT: [[Index]]
