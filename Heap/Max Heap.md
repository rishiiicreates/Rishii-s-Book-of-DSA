# Max Heap Basics

## Problem Statement
- use the C++ standard library to create and robustly manipulate a Max Heap seamlessly in-place.

## Approach / Intuition
- while custom implementations are functional, the `<algorithm>` library heavily provides native methods like `make_heap`, `push_heap`, and `pop_heap`. Utilizing these directly on standard vectors guarantees highly optimized, inline [[Heap Operations]] without structural overhead.

## Time & Space Complexity
- **[[time Complexity]]:** O(log N) for push/pop, O(N) for make_heap
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
void demonstrateMaxHeap() {
    vector<int> v = {3, 1, 4, 1, 5, 9};
    make_heap(v.begin(), v.end());
    
    v.push_back(6);
    push_heap(v.begin(), v.end());
    
    pop_heap(v.begin(), v.end());
    int max_val = v.back();
    v.pop_back();
}
```

## New Keywords / STL Used
- `std::make_heap`, `std::push_heap`, `std::pop_heap`

## Edge Cases
- operating natively on purely empty vectors or arrays initialized with 1 element.

NEXT: [[Index]]
