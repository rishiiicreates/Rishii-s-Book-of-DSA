---
type: concept
tags: [hashing, cpp, collision-resolution]
date: 2026-06-30
---
# Quadratic Probing

## Problem Statement
Implement an [[Open Addressing]] collision resolution strategy that mitigates the primary clustering issue inherent in [[Linear Probing]].

---

## Approach: Quadratic Jump Sequences

Instead of checking adjacent slots $(i+1, i+2, i+3)$, Quadratic Probing increases the step size quadratically. 

1. **Probe Sequence:** The $i$-th probe for key $K$ is defined mathematically as:
   $$ P(K, i) = (h(K) + c_1 \cdot i + c_2 \cdot i^2) \bmod M $$
   Typically, we use $c_1 = 0, c_2 = 1$, giving the simplified sequence:
   $$ P(K, i) = (h(K) + i^2) \bmod M $$
   This means the offsets are $1, 4, 9, 16 \dots$
2. **Mitigating Primary Clustering:** Because the step size changes, two keys that hash to adjacent locations will follow completely diverging probe paths, breaking up the continuous blocks (primary clusters) seen in linear probing.
3. **The Cycle Constraint:** A major mathematical danger of quadratic probing is that it might not evaluate every slot in the table. If $M$ is prime and $\alpha \le 0.5$, it is mathematically guaranteed to find an empty slot. If $M$ is not prime (e.g., a power of 2), the sequence will loop prematurely and fail to find empty slots even when the table is mostly empty.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

class QuadraticProbingHash {
private:
    vector<int> table;
    int capacity;
    const int EMPTY = -1;

    int hash(int key) {
        return key % capacity;
    }

public:
    QuadraticProbingHash(int cap) : capacity(cap), table(cap, EMPTY) {}

    void insert(int key) {
        int index = hash(key);
        int i = 0;
        
        // Using P(K, i) = (H(K) + i^2) % M
        while (table[(index + i * i) % capacity] != EMPTY) {
            if (table[(index + i * i) % capacity] == key) return; // Duplicate
            
            i++;
            // If i reaches capacity, we might be trapped in a cycle
            if (i == capacity) return; 
        }
        
        table[(index + i * i) % capacity] = key;
    }
    
    // Search and Delete follow the identical (index + i * i) % capacity sequence.
};
```

> [!important]
> **Secondary Clustering:** While quadratic probing solves primary clustering, it suffers from secondary clustering: if two different keys hash to the *exact same* initial bucket $h(K_1) = h(K_2)$, they will follow the *exact same* quadratic probe sequence. This is solved by Double Hashing.

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ expected if $\alpha \le 0.5$. Worst-case $O(N)$ for search/insertion.
- **Space Complexity:** $O(M)$ where $M$ is the table capacity.
