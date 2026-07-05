# Multiply with Adjacent

## Problem Statement
- given an array $A$ of size $N$, replace every element $A[i]$ with the product of its previous and next elements, i.e., $A[i] = A[i-1] \times A[i+1]$.
- for the first element, replace it with $A[0] \times A[1]$. For the last element, replace it with $A[N-1] \times A[N-2]$.
- all replacements must happen simultaneously (i.e., using the original values of adjacent elements).
- *example:* `[2, 3, 4, 5, 6]` becomes `[6, 8, 15, 24, 30]`.


## Approach: Stateful Variable Tracking

- a naive approach would be to create a new array, compute the products, and copy them back, resulting in an $O(N)$ space complexity.

- to solve this in $O(1)$ auxiliary space, we must traverse the array and update elements in place. However, updating $A[i]$ overwrites the value needed to compute $A[i+1]$.
- we can mathematically resolve this by tracking the original value of the *previous* element in a separate variable.

- store the original $A[0]$ in a variable `prev`.
- compute the new $A[0] = A[0] \times A[1]$.
- iterate from $i = 1$ to $N-2$:
   - temporarily store the original $A[i]$ in a variable `curr`.
   - update $A[i] = \text{prev} \times A[i+1]$.
   - update `prev = curr` for the next iteration.
- finally, update $A[N-1] = \text{prev} \times A[N-1]$.


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

void multiplyWithAdjacent(vector<int>& arr) {
    int n = arr.size();
    
    // Base cases: 0 or 1 element, no adjacent elements exist
    if (n <= 1) return;
    
    // Store the original first element
    int prev = arr[0];
    
    // Update the first element
    arr[0] = arr[0] * arr[1];
    
    // Traverse intermediate elements
    for (int i = 1; i < n - 1; ++i) {
        int curr = arr[i];              // Store original A[i]
        arr[i] = prev * arr[i + 1];     // Compute new A[i] using original A[i-1]
        prev = curr;                    // Carry forward original A[i] as the next prev
    }
    
    // Update the last element using the carried over prev
    arr[n - 1] = prev * arr[n - 1];
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ because we traverse the array exactly once.
- **space Complexity:** $O(1)$ since we only use a few integer variables (`prev` and `curr`) to maintain state.

> [!important]
> **Integer Overflow:** The product of two adjacent elements can easily exceed the 32-bit integer limit. In a rigorous environment or competitive programming context, change the array type to `vector<long long>` to prevent overflow issues.

NEXT: [[Index]]
