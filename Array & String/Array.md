---
type: concept
tags: [array, memory, cpp, basics]
date: 2026-07-01
---
# Array

## Problem Statement
Understand the fundamental concepts of the Array data structure, specifically its contiguous memory allocation, indexing mechanism, and basic operations.

---

## Approach: Contiguous Memory Allocation

An **Array** is a linear data structure that stores elements of the same data type in contiguous memory locations. Because the memory is contiguous, any element can be accessed directly using its index.

If the base address of the array is $B$, the size of each element is $S$, and the array uses $0$-based indexing, the memory address of the $i$-th element is given by:
$$ \text{Address}(i) = B + i \times S $$

Because this requires a single arithmetic operation, accessing an element takes constant time. Arrays form the foundational building block for many other complex data structures (like Strings, Matrices, and Hash Tables).

### Fixed vs Dynamic Arrays
In C++, a standard array (`int arr[10]`) has a fixed size allocated on the stack or heap. A `std::vector` is a dynamic array that automatically resizes itself when it runs out of capacity (usually by doubling its size).

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    // Dynamic array initialization
    vector<int> arr = {1, 2, 3, 4, 5};
    
    // O(1) amortized insertion at the end
    arr.push_back(6);
    
    // O(1) access
    cout << "Element at index 2: " << arr[2] << "\n";
    
    // Traversal
    for(int x : arr) {
        cout << x << " ";
    }
    cout << "\n";
    
    return 0;
}
```

---

## Complexity Analysis
- **Time Complexity:** 
  - Access: $O(1)$ due to contiguous memory allocation.
  - Insertion/Deletion at the end: $O(1)$ amortized.
  - Insertion/Deletion in the middle: $O(N)$ because elements must be shifted.
- **Space Complexity:** $O(N)$ where $N$ is the number of elements in the array.

> [!warning]
> **Out of Bounds Access:** Accessing an array outside its allocated memory bounds in C++ leads to Undefined Behavior (UB). Always ensure indices are strictly in the range $[0, N-1]$.
