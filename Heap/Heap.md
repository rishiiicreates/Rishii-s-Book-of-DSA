---
type: concept
tags: [heap, priority-queue, tree, fundamental, cpp]
date: 2026-06-30
---
# Heap Data Structure

## Mathematical Definition
A **Heap** is a specialized tree-based data structure that satisfies two fundamental mathematical invariants:
1. **Structural Invariant (Complete Binary Tree):** Every hierarchical level of the tree must be fully populated with nodes, with the strict exception of the lowest level, which must be populated contiguous from left to right.
2. **Order Invariant (Heap Property):**
   - **Max Heap:** For any given node $N$, the key of $N$ is mathematically greater than or equal to the keys of its topological children: $\forall C \in \text{Children}(N), \text{key}(N) \ge \text{key}(C)$.
   - **Min Heap:** For any given node $N$, the key of $N$ is mathematically less than or equal to the keys of its topological children: $\forall C \in \text{Children}(N), \text{key}(N) \le \text{key}(C)$.

---

## Array Representation
Because a heap is strictly a Complete Binary Tree, pointers (`left`, `right`) are theoretically redundant. The topology maps perfectly onto a contiguous 0-indexed array, providing massive cache locality advantages.

For a node located at index $i$:
- **Parent:** $\lfloor \frac{i - 1}{2} \rfloor$
- **Left Child:** $2i + 1$
- **Right Child:** $2i + 2$

> [!important]
> The absolute maximum or minimum element is unconditionally located at the structural root: index $0$. However, unlike a Binary Search Tree, a heap provides *zero* mathematical guarantees regarding the lateral ordering of siblings. 

---

## Fundamental Operations and Complexities

| Operation | Concept | Complexity |
| :--- | :--- | :--- |
| **Get Max / Get Min** | Retrieve `array[0]` | $O(1)$ |
| **Insertion** | Append at end, then `heapifyUp()` | $O(\log N)$ |
| **Deletion (Extract)**| Swap root with last element, pop, then `heapifyDown()` | $O(\log N)$ |
| **Heapify (Build)** | Bottom-up structural validation | $O(N)$ |

### Mathematical Proof of $O(N)$ Build Heap
Building a heap by sequentially inserting $N$ elements takes $O(N \log N)$. However, utilizing the bottom-up `heapifyDown()` algorithm on an unsorted array takes strictly $O(N)$. 

Proof: At height $h$ (where leaves are $h=0$), there are at most $\lceil \frac{N}{2^{h+1}} \rceil$ nodes. The cost to heapify a node at height $h$ is bound by $c \cdot h$. 
Total work: $W = \sum_{h=0}^{\lfloor \log_2 N \rfloor} \frac{N}{2^{h+1}} \cdot c \cdot h$. This infinite geometric series mathematically converges to a strict upper bound of $O(N)$.

---

## Standard Library (C++)

In C++, the heap data structure is abstracted via the `std::priority_queue` container adapter.

```cpp
#include <queue>
#include <vector>

// Max Heap (Default)
std::priority_queue<int> maxHeap; 

// Min Heap (Explicit Comparator)
std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
```
