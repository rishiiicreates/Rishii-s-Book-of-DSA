# Two-Pointer Technique

## Problem Statement
- design an algorithmic pattern to process sequential data structures (arrays, strings, linked lists) efficiently by tracking two distinct indices simultaneously.


## Approach: Algorithmic Paradigms

- the Two-Pointer technique mathematically reduces the search space of nested loops, often optimizing $O(N^2)$ brute-force solutions down to $O(N)$ or $O(N \log N)$. It typically manifests in three primary topological paradigms:

- **opposite Ends (Collision / Convergence):**
   - one pointer starts at index $0$ (`left`), and the other starts at $N-1$ (`right`).
   - at each step, a mathematical condition determines whether `left` increments or `right` decrements.
   - the loop terminates when `left \ge right`.
   - *use Case:* Searching for pairs in sorted arrays, reversing arrays, palindrome verification.

- **same Direction (Fast and Slow / Tortoise and Hare):**
   - both pointers start at index $0$.
   - the `fast` pointer advances at a faster rate (e.g., $+2$ steps) or unconditionally, while the `slow` pointer advances at a slower rate (e.g., $+1$ step) or only when a specific condition is met.
   - *use Case:* Cycle detection in graphs/linked lists, removing duplicates in-place, finding the midpoint of a linked list.

- **sliding Window:**
   - both pointers start at index $0$ and move in the same direction, bounding a subarray $[L, R]$.
   - $r$ expands the window to satisfy a condition; $L$ shrinks the window to restore validity or optimize the condition.
   - *use Case:* Subarray sums, longest substrings with constraints.


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


## Complexity Analysis
- **time Complexity:** $O(N)$. Each pointer moves strictly in one direction without backtracking. The distance between them shrinks by exactly $1$ at each step, taking at most $N$ steps.
- **space Complexity:** $O(1)$. We only maintain two integer scalar variables.

NEXT: [[Index]]
