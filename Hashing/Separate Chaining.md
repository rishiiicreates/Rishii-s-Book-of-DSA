---
type: concept
tags: [hashing, cpp, collision-resolution]
date: 2026-06-30
---
# Separate Chaining

## Problem Statement
Implement a collision resolution strategy for a hash table that handles high load factors gracefully and never mathematically "fills up".

---

## Approach: Bucket Lists

Instead of restricting each array slot to a single element, Separate Chaining allows each index (bucket) in the hash table to store a collection of elements, typically implemented as a [[Linked List]] or dynamic array.

1. **Mapping:** The hash function $h(K)$ maps the key to an index $0 \le \text{index} < M$.
2. **Insertion:** Calculate `index = h(K)`. Traverse the list at `table[index]` to check for duplicates. If none exist, append $K$ to the list.
3. **Search/Deletion:** Calculate `index = h(K)`. Traverse the list at `table[index]` to find $K$. This traversal takes time proportional to the length of the chain.
4. **Expected Length:** If a uniform hash function distributes $N$ elements across $M$ buckets, the expected length of any chain is exactly the load factor $\alpha = \frac{N}{M}$. 

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class SeparateChainingHash {
private:
    int capacity;
    vector<vector<int>> table; // Using dynamic arrays instead of Linked Lists for cache locality

    int hash(int key) const {
        return key % capacity;
    }

public:
    SeparateChainingHash(int cap) : capacity(cap), table(cap) {}

    void insert(int key) {
        int index = hash(key);
        
        // Check for duplicates
        for (int num : table[index]) {
            if (num == key) return;
        }
        
        // Append to the chain
        table[index].push_back(key);
    }
    
    bool search(int key) const {
        int index = hash(key);
        for (int num : table[index]) {
            if (num == key) return true;
        }
        return false;
    }
    
    void remove(int key) {
        int index = hash(key);
        auto it = find(table[index].begin(), table[index].end(), key);
        if (it != table[index].end()) {
            // Fast removal from vector by swapping with the last element
            *it = table[index].back();
            table[index].pop_back();
        }
    }
};
```

> [!tip]
> In modern C++, utilizing `std::vector` for the chains is often faster than `std::list` due to spatial locality and CPU cache prefetching, even though inserting/deleting in the middle of a vector is theoretically $O(K)$. The chains are usually kept so small (e.g., size $\le 4$) that contiguous memory dominates asymptotic overhead.

---

## Complexity Analysis
- **Time Complexity:** $O(1 + \alpha)$ expected for all operations, where $\alpha$ is the load factor. Worst-case $O(N)$ if all keys hash to the exact same bucket.
- **Space Complexity:** $O(N + M)$ for the table of size $M$ and the $N$ total elements stored across all vectors.
