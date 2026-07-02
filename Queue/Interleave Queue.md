---
type: concept
tags: [queue, stack, cpp]
date: 2026-06-30
---
# Interleave the First Half of the Queue with Second Half

## Problem Statement
Given a Queue $Q$ of integers of even length $N$, mathematically interleave the first half of the queue with the second half.

*Example:* $Q = [11, 12, 13, 14, 15, 16, 17, 18, 19, 20]$
*Result:* $Q = [11, 16, 12, 17, 13, 18, 14, 19, 15, 20]$

---

## Approach: Auxiliary Queue Partitioning

The most intuitive mathematical reduction involves partitioning the original queue $Q$ into two distinct subsets and subsequently interleaving them.

1. **Partitioning:** Initialize an auxiliary queue $Q_{aux}$. Since $N$ is even, we can precisely dequeue the first $\frac{N}{2}$ elements from $Q$ and enqueue them into $Q_{aux}$.
   - *State:* $Q_{aux}$ contains the first half. $Q$ contains the second half.
2. **Interleaving:** While $Q_{aux}$ is not empty:
   - Dequeue the front element from $Q_{aux}$ and enqueue it into $Q$. (This places the first-half element at the back).
   - Dequeue the front element from $Q$ (which is a second-half element) and immediately enqueue it back into $Q$.

### Alternative Approach: Auxiliary Stack

If restricted to using a [[Stack]] instead of an auxiliary queue, the problem requires multiple topological inversions since a stack naturally reverses order.
1. Push first half into Stack, pop and enqueue to $Q$ (now at the back, but reversed).
2. Shift the first half to the back again.
3. Push first half into Stack (restoring original relative order), then pop and interleave with $Q$.
*This proves that using a Queue simplifies the permutations mathematically.*

---

## Code Implementation (Auxiliary Queue)

```cpp
#include <queue>
using namespace std;

void interleaveQueue(queue<int>& q) {
    if (q.size() % 2 != 0) return; // N must be strictly even
    
    int halfSize = q.size() / 2;
    queue<int> aux;
    
    // Step 1: Extract first half into auxiliary queue
    for (int i = 0; i < halfSize; ++i) {
        aux.push(q.front());
        q.pop();
    }
    
    // Step 2: Interleave elements mathematically
    while (!aux.empty()) {
        // Enqueue from first half
        q.push(aux.front());
        aux.pop();
        
        // Enqueue from second half
        q.push(q.front());
        q.pop();
    }
}
```

> [!important]
> For the mathematical permutation to map cleanly without offset issues, $N$ must strictly be an even parity integer. If $N$ is odd, the definition of "first half" is ambiguous and requires domain-specific rounding rules.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We traverse the first half in $O(N/2)$ and the entire interleaving process operates in $O(N)$. The net time complexity is strictly linear.
- **Space Complexity:** $O(N)$. The auxiliary queue requires memory proportional to $\frac{N}{2}$, which normalizes to $O(N)$ asymptotically.
