---
type: concept
tags: [hashing, cpp, math, optimization]
date: 2026-07-01
---
# Most Frequent Element

## Problem Statement
Given an array sequence $A$ of $N$ integers, mathematically identify the element $M$ that constitutes the absolute statistical mode (the element with the highest occurrence frequency). If the distribution contains multimodal ties, return any of the modal elements.

---

## Approach: Single-Pass Global Maximum Extraction

While a standard two-pass algorithm generates the full frequency distribution hash map and subsequently scans it for the maximal node, we can fuse these operations. 

We apply an iterative Single-Pass state machine.
We maintain the hash map to compute frequencies in real-time. Simultaneously, we define two scalar state variables: `max_frequency` and `modal_element`.
Upon incrementing the frequency map for the current array scalar $A_i$:
1. We evaluate if the newly updated $f(A_i)$ exceeds the global `max_frequency`.
2. If the inequality holds, we mathematically overwrite `max_frequency` and register $A_i$ as the current provisional `modal_element`.

This avoids a secondary iteration over the hash map entirely, optimizing cache locality and reducing operations by a strict mathematical constant factor.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_map>

using namespace std;

int mostFrequent(const vector<int>& arr) {
    unordered_map<int, int> freq_map;
    
    int max_freq = 0;
    int modal_element = -1; 
    
    for (int num : arr) {
        // Increment distribution
        freq_map[num]++;
        
        // Dynamic global peak evaluation
        if (freq_map[num] > max_freq) {
            max_freq = freq_map[num];
            modal_element = num;
        }
    }
    
    return modal_element;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ amortized single pass. The map insertions resolve in $O(1)$ amortized average time. The global peak evaluation is exactly $O(1)$.
- **Space Complexity:** $O(U)$, where $U$ is the cardinality of unique integers within the sequence. Upper bounded by $O(N)$.

> [!warning]
> **Boyer-Moore Limitations:** If the mathematical constraints guarantee that the modal element appears strictly more than $\lfloor N / 2 \rfloor$ times, the `std::unordered_map` can be entirely replaced by the Boyer-Moore Majority Vote Algorithm, crushing the space complexity to $O(1)$. However, if the threshold is not guaranteed, the Hash Map methodology remains the universally robust standard.
