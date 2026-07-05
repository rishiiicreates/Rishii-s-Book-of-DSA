# Count Frequencies

## Problem Statement
- given a discrete mathematical sequence (an array of integers), compute the precise occurrence frequency distribution for all unique elements within the domain.


## Approach: Hash Map Distribution Accumulation

- to establish the frequency spectrum of the array, we utilize a Hash Table. The hash table acts as a mathematical dictionary defining a mapping $f: E \to \mathbb{Z}^+$, where $E$ is the set of unique elements, and $f(x)$ is the occurrence count.

- we execute a single linear pass over the sequence. For every scalar value $V_i$:
- we invoke the hash function $h(V_i)$ to map the value to a memory bucket.
- we increment the value stored at that bucket.

- in C++, `std::unordered_map` implicitly initializes missing integral keys with the zero state `0`. Thus, `freq[num]++` securely initializes and increments the frequency simultaneously without requiring an explicit presence check.


## Code Implementation

```cpp
#include <vector>
#include <unordered_map>
#include <iostream>

using namespace std;

unordered_map<int, int> countFrequencies(const vector<int>& arr) {
    unordered_map<int, int> freq_map;
    
    // Accumulate the frequency distribution
    for (int num : arr) {
        freq_map[num]++;
    }
    
    return freq_map;
}

// Example usage iteration:
// for (const auto& [element, frequency] : freq_map) { ... }
```


## Complexity Analysis
- **time Complexity:** $O(N)$ amortized. The `std::unordered_map` utilizes Robin Hood or Chaining structures yielding $O(1)$ average insertion time. In pathological collision scenarios, this degrades to $O(N^2)$, though standard `std::hash` is reasonably resilient.
- **space Complexity:** $O(U)$ auxiliary space, where $U$ is the exact cardinality of unique elements in the array. Bounded by $O(N)$.

> [!tip]
> **Deterministic Ordering:** If the algorithmic constraints demand that the frequency output be sorted by the keys (the elements themselves), one must theoretically shift from `std::unordered_map` to `std::map` (a Red-Black Tree implementation). This mathematically alters the insertion and traversal bounds to $O(N \log N)$.

NEXT: [[Index]]
