---
type: concept
tags: [two-pointer, cpp, algorithm-pattern]
date: 2026-06-30
---
# Two-Pointer Technique

## Problem Statement
Design an algorithmic pattern to process sequential data structures (arrays, strings, linked lists) efficiently by tracking two distinct indices simultaneously.

---

## Approach: Algorithmic Paradigms

The Two-Pointer technique mathematically reduces the search space of nested loops, often optimizing $O(N^2)$ brute-force solutions down to $O(N)$ or $O(N \log N)$. It typically manifests in three primary topological paradigms:

1. **Opposite Ends (Collision / Convergence):**
   - One pointer starts at index $0$ (`left`), and the other starts at $N-1$ (`right`).
   - At each step, a mathematical condition determines whether `left` increments or `right` decrements.
   - The loop terminates when `left \ge right`.
   - *Use Case:* Searching for pairs in sorted arrays, reversing arrays, palindrome verification.

2. **Same Direction (Fast and Slow / Tortoise and Hare):**
   - Both pointers start at index $0$.
   - The `fast` pointer advances at a faster rate (e.g., $+2$ steps) or unconditionally, while the `slow` pointer advances at a slower rate (e.g., $+1$ step) or only when a specific condition is met.
   - *Use Case:* Cycle detection in graphs/linked lists, removing duplicates in-place, finding the midpoint of a linked list.

3. **Sliding Window:**
   - Both pointers start at index $0$ and move in the same direction, bounding a subarray $[L, R]$.
   - $R$ expands the window to satisfy a condition; $L$ shrinks the window to restore validity or optimize the condition.
   - *Use Case:* Subarray sums, longest substrings with constraints.

---

## Code Implementation (Opposite Ends Template)

```cpp
#include <vector>
using namespace std;

void twoPointerConvergence(const vector<int>& arr) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left < right) {
        // Evaluate the pair (arr[left], arr[right])
        long long current_state = arr[left] + arr[right]; 
        
        if (/* Condition met */ current_state == 0) {
            // Process success
            break;
        } else if (/* Needs larger value */ current_state < 0) {
            left++;
        } else {
            // Needs smaller value
            right--;
        }
    }
}
```

> [!important]
> For the convergence paradigm to be mathematically valid, the array **must be sorted**. If the array is unsorted, advancing `left` or decrementing `right` does not provide a monotonic guarantee regarding the change in the state function (e.g., the sum).

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. Each pointer moves strictly in one direction without backtracking. The distance between them shrinks by exactly $1$ at each step, taking at most $N$ steps.
- **Space Complexity:** $O(1)$. We only maintain two integer scalar variables.
