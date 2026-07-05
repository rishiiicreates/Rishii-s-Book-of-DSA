# Cycle Sort

## Problem Statement
- implement the Cycle Sort algorithm on an array $A$. Cycle Sort is an unstable sorting algorithm that minimizes the number of memory writes, making it mathematically optimal for systems where writing to memory is highly expensive (e.g., EEPROM, Flash).

- *example:* $A = [10, 5, 2, 3]$
- *result:* $[2, 3, 5, 10]$


## Approach: Mathematical Permutation Cycles

- cycle Sort decomposes the array into disjoint cycles representing the underlying permutation that sorts the array.

- start at index `start`. The element `item = A[start]` is the beginning of a cycle.
- find the correct position for `item`. This is done by counting how many elements in the entire array (from `start + 1` to $N-1$) are strictly smaller than `item`. Let this count be $C$. The correct position is `pos = start + C`.
- if `pos == start`, the element is already in the correct place. Move to the next `start`.
- if there are duplicates, we may need to advance `pos` to skip over them: `while (item == A[pos]) pos++;`.
- swap `item` with `A[pos]`. Now `A[pos]` holds the correct element, but the element previously at `A[pos]` (which is now in `item`) is displaced!
- repeat the process to find the correct position for the newly displaced `item`. Continue this sequence until the cycle closes (i.e., `pos == start`), meaning the originally displaced element has returned home.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

void cycleSort(vector<int>& arr) {
    int n = arr.size();
    
    // Loop over the array to find cycles
    for (int cycle_start = 0; cycle_start <= n - 2; ++cycle_start) {
        int item = arr[cycle_start];
        int pos = cycle_start;
        
        // Count how many elements are smaller than `item`
        for (int i = cycle_start + 1; i < n; ++i) {
            if (arr[i] < item) pos++;
        }
        
        // If it's already in the correct position, continue
        if (pos == cycle_start) continue;
        
        // Skip over duplicates
        while (item == arr[pos]) pos++;
        
        // Put `item` in its correct position, displacing the element there
        if (pos != cycle_start) {
            swap(item, arr[pos]);
        }
        
        // Resolve the rest of the cycle
        while (pos != cycle_start) {
            pos = cycle_start;
            
            // Find the correct position for the newly displaced `item`
            for (int i = cycle_start + 1; i < n; ++i) {
                if (arr[i] < item) pos++;
            }
            
            // Skip over duplicates
            while (item == arr[pos]) pos++;
            
            // Swap into position
            if (item != arr[pos]) {
                swap(item, arr[pos]);
            }
        }
    }
}
```

> [!important]
> Cycle Sort performs exactly zero or one write per element to reach its final sorted position. This mathematically guarantees the absolute minimum number of memory writes to sort an array.


## Complexity Analysis
- **time Complexity:** $O(N^2)$ in all cases. For every element, we scan the rest of the array to count smaller elements.
- **space Complexity:** $O(1)$. Strictly in-place with a few variables for counting.

NEXT: [[Index]]
