# Min Heap Basics

## Problem Statement
- implement a custom Min Heap Data Structure that supports fast insertion and extraction of the minimal element.

## Approach / Intuition
- a Min Heap is structurally a complete binary tree where every node evaluates as strictly smaller than its children. Implementing it sequentially using a flat array requires manual `heapify` operations recursively pushing new elements up or aggressively bubbling heavy nodes down, forming the foundation of native [[Heap Operations]].

## Time & Space Complexity
- **[[time Complexity]]:** O(log N) for push/pop, O(1) for top
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
class MinHeap {
    vector<int> heap;
    void heapifyUp(int i) {
        while (i != 0 && heap[(i - 1) / 2] > heap[i]) {
            swap(heap[i], heap[(i - 1) / 2]);
            i = (i - 1) / 2;
        }
    }
    void heapifyDown(int i) {
        int smallest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        if (left < heap.size() && heap[left] < heap[smallest]) smallest = left;
        if (right < heap.size() && heap[right] < heap[smallest]) smallest = right;
        if (smallest != i) {
            swap(heap[i], heap[smallest]);
            heapifyDown(smallest);
        }
    }
public:
    void push(int val) {
        heap.push_back(val);
        heapifyUp(heap.size() - 1);
    }
    void pop() {
        if (heap.empty()) return;
        heap[0] = heap.back();
        heap.pop_back();
        heapifyDown(0);
    }
    int top() {
        return heap.empty() ? -1 : heap[0];
    }
};
```

## New Keywords / STL Used
- `class`

## Edge Cases
- popping elements iteratively from a completely empty heap instance.

NEXT: [[Index]]
