# Minimum Deques to Sort Array

## Problem Statement
- given an array of $N$ distinct integers, you possess an infinite supply of empty double-ended queues ([[Deque]]s). You mathematically extract elements from the original array strictly from left to right (index $0$ to $N-1$) and place them into the deques. For any deque, you may append an element to either its front or its back.

- finally, you must concatenate all the non-empty deques in some arbitrary sequential order to produce a strictly sorted, monotonically increasing unified array. What is the mathematically minimum number of deques required to fulfill this condition?


## Approach: Coordinate Compression and LIS Topological Equivalence

- this problem poses a topological transformation challenge. For a set of elements placed within a single deque to remain strictly sorted when extracted, the sequence in which they are inserted must mathematically mirror an expanding sorted envelope (e.g., if inserting $5$, the next valid bounds for the deque are strictly $<5$ via `push_front` or $>5$ via `push_back`).

- if we map the elements to their ultimate strictly sorted positions (Coordinate Compression), the sequence of indices at which consecutive numbers appear in the original array forms alternating increasing and decreasing contiguous subsequences.

- **mapping Sequence Inversions:** Let $pos[x]$ represent the original spatial index of element $x$ in the sorted domain.
- **deque Capability Analysis:** A single deque can mathematically satisfy exactly one decreasing sequence of original indices followed seamlessly by one increasing sequence of original indices.
   - specifically, if we traverse elements in ascending sorted order:
   - when $pos[i] < pos[i-1]$, we logically place element $i$ at the *front* of the deque.
   - when $pos[i] > pos[i-1]$, we logically place element $i$ at the *back* of the deque.
   - the sequence breaks (necessitating a new deque) exactly when the topological direction changes from **increasing back to decreasing**.
- **execution Logic:**
   - sort the elements alongside their original original indices (e.g., using `std::pair`).
   - iterate through the sorted sequence. Track the derivative direction of the original indices.
   - if the previous movement was "increasing" ($pos[i-1] < pos[i]$) but the current movement dictates a "decreasing" jump ($pos[i] < pos[i-1]$), the mathematical capacity of the current deque is completely violated. We must allocate a new deque. Increment the deque counter.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int minDequesToSort(const vector<int>& arr) {
    int n = arr.size();
    if (n == 0) return 0;
    
    // Vector of pairs: {value, original_index}
    vector<pair<int, int>> sorted_arr(n);
    for (int i = 0; i < n; ++i) {
        sorted_arr[i] = {arr[i], i};
    }
    
    // Sort fundamentally by value
    sort(sorted_arr.begin(), sorted_arr.end());
    
    int deque_count = 1;
    bool is_increasing = false;
    
    for (int i = 1; i < n; ++i) {
        int current_pos = sorted_arr[i].second;
        int prev_pos = sorted_arr[i - 1].second;
        
        if (current_pos > prev_pos) {
            // Direction is rightward (push_back behavior)
            is_increasing = true;
        } else {
            // Direction is leftward (push_front behavior)
            // If we were previously increasing, we cannot switch back to leftward 
            // without corrupting the topological sort invariant of the current deque.
            if (is_increasing) {
                deque_count++;
                is_increasing = false;
            }
        }
    }
    
    return deque_count;
}
```

> [!important]
> The algorithm does not explicitly simulate placing elements into physical `std::deque` objects. Simulating the process yields no computational benefit. Recognizing the mathematical pattern of index directionality reduces the entire problem to simple state transition tracking.


## Complexity Analysis
- **time Complexity:** $O(N \log N)$. Constructing the augmented pair array operates in $O(N)$. Sorting it strictly dictates the asymptotic upper bound of $O(N \log N)$. The subsequent linear scan is bound by $O(N)$.
- **space Complexity:** $O(N)$ auxiliary space allocated for the `sorted_arr` mathematical mapping structure.

NEXT: [[Index]]
