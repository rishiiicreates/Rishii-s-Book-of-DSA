---
type: concept
tags: [heap, min-heap, greedy, map, cpp]
date: 2026-07-01
---
# Hand of Straights

## Problem Statement
Given an integer array `hand` of size $N$ and an integer group size `groupSize`, strictly determine if it is mathematically possible to structurally partition the array into disjoint sequential permutations of identical scalar length `groupSize`. A valid permutation structurally demands consecutive scalars: $(X, X+1, X+2, \dots, X+W-1)$.

---

## Approach: Min-Heap Driven Topological Anchoring

This is a structural permutation binding problem. 
If $N \pmod{\text{groupSize}} \neq 0$, a mathematical partition is theoretically impossible.
For the sequence mapping, we must greedily construct permutations anchored on the absolute lowest available scalar.

We deploy a **Min-Heap (Priority Queue)** over a Hash Map frequencies:
1. Map the topological frequency of every scalar.
2. Inject all unique scalars into a Min-Heap to guarantee absolute ascending monotonic extraction.
3. While the Min-Heap is populated:
   - The geometric `root` (absolute minimum) MUST structurally act as the anchor of a new consecutive sequence.
   - We geometrically iterate from $X$ to $X + \text{groupSize} - 1$.
   - If any required sequential scalar mathematically does not exist (or its frequency is 0), the topological partition immediately collapses $\to$ `false`.
   - Mutate the sequence frequencies. If a scalar frequency bounds to zero, we mathematically demand it matches the exact root of the Min-Heap. If it does, `pop()`. If it hits zero prematurely (nested), it implies a topological hole, terminating the sequence validity.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_map>
#include <queue>

using namespace std;

bool isNStraightHand(vector<int>& hand, int groupSize) {
    if (hand.size() % groupSize != 0) return false;
    
    // Structural Frequency Mapping
    unordered_map<int, int> count;
    priority_queue<int, vector<int>, greater<int>> min_heap;
    
    for (int card : hand) {
        count[card]++;
    }
    
    // Inject unique geometry to Min-Heap
    for (auto const& pair : count) {
        min_heap.push(pair.first);
    }
    
    // Iterative Sequence Construction
    while (!min_heap.empty()) {
        int first = min_heap.top();
        
        // Construct strict sequential progression
        for (int i = first; i < first + groupSize; i++) {
            if (count.find(i) == count.end() || count[i] == 0) {
                return false; // Topological sequence broken
            }
            
            count[i]--;
            
            // Absolute validation: only the topological head should exhaust
            if (count[i] == 0) {
                if (min_heap.top() != i) {
                    return false; // Topological hole isolation violation
                }
                min_heap.pop();
            }
        }
    }
    
    return true;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N \log U)$, where $U$ denotes the cardinality of unique scalars, governed entirely by the Min-Heap topological operations. The internal geometric sequencing strictly bounds to total element extraction $O(N)$.
- **Space Complexity:** $O(U)$ geometric limit for mapping frequencies and storing elements inside the structural heap.

> [!tip]
> **Ordered Map Isomorphism:** A `std::map<int, int>` naturally binds keys in strict ascending monotonic order. Iterating the map and recursively collapsing sequences identically satisfies the constraints in $O(N \log U)$ time, effectively bypassing explicit Heap manipulation while maintaining identical theoretical properties.
