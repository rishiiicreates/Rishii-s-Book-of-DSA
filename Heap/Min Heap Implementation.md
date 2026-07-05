# Min Heap Implementation

## Problem Statement
- mathematically construct a dynamic **Min Heap** from scratch using a contiguous array representation. Implement the core operations: `insert()`, `extractMin()`, and `heapify()`.


## Structural Logic

- a Min Heap is structurally enforced by two cascading operations:
- **heapify-Up (Percolate Up):** Invoked during `insert()`. A new element is appended to the absolute end of the array (preserving the Complete Binary Tree property). If this node violates the Min Heap order ($Node < Parent$), it is mathematically swapped upwards until the invariant is restored.
- **heapify-Down (Percolate Down):** Invoked during `extractMin()`. The root (minimum element) is overwritten by the last element in the array, and the array shrinks. Because this new root likely violates the Min Heap order ($Node > Children$), it is mathematically swapped downwards with its *smallest child* until the invariant is restored.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>
using namespace std;

class MinHeap {
private:
    vector<int> heap;
    
    // Topological address calculators
    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return (2 * i) + 1; }
    int rightChild(int i) { return (2 * i) + 2; }
    
    // O(log N) upward structural validation
    void heapifyUp(int index) {
        while (index > 0 && heap[parent(index)] > heap[index]) {
            swap(heap[parent(index)], heap[index]);
            index = parent(index);
        }
    }
    
    // O(log N) downward structural validation
    void heapifyDown(int index) {
        int smallest = index;
        int left = leftChild(index);
        int right = rightChild(index);
        int size = heap.size();
        
        // Mathematical evaluation of minimal path
        if (left < size && heap[left] < heap[smallest]) {
            smallest = left;
        }
        if (right < size && heap[right] < heap[smallest]) {
            smallest = right;
        }
        
        // Structural violation detected
        if (smallest != index) {
            swap(heap[index], heap[smallest]);
            heapifyDown(smallest); // Recursive propagation
        }
    }

public:
    // O(log N)
    void insert(int value) {
        heap.push_back(value);
        heapifyUp(heap.size() - 1);
    }
    
    // O(log N)
    int extractMin() {
        if (heap.empty()) {
            throw out_of_range("Heap underflow");
        }
        
        int root = heap[0];
        // Relocate terminal node to root to preserve structural completeness
        heap[0] = heap.back();
        heap.pop_back();
        
        if (!heap.empty()) {
            heapifyDown(0);
        }
        return root;
    }
    
    // O(1)
    int getMin() const {
        if (heap.empty()) {
            throw out_of_range("Heap underflow");
        }
        return heap[0];
    }
};
```

> [!tip]
> A `Max Heap` implementation is topologically identical. The only mathematical alteration required is inverting the relational operators: `heapifyUp` triggers when $Parent < Node$, and `heapifyDown` seeks the *largest* child instead of the smallest.


## Complexity Analysis
- **time Complexity:**
  - `insert`: $O(\log N)$ due to the upward path bounded by tree height.
  - `extractMin`: $O(\log N)$ due to the downward path bounded by tree height.
  - `getMin`: $O(1)$.
- **space Complexity:** $O(N)$ dynamic memory auxiliary overhead to store the $N$ elements within the `std::vector`. Recursive `heapifyDown` incurs an implicit $O(\log N)$ stack overhead, which can trivially be optimized to $O(1)$ iteratively.

NEXT: [[Index]]
