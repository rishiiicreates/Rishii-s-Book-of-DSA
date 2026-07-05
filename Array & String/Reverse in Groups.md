# Reverse in Groups

## Problem Statement
- given an array $A$ of size $N$, reverse every sub-array of length $K$. If the final remaining elements in the array form a group of size less than $K$, they should all be reversed as well.

- *example:* $A = [1, 2, 3, 4, 5, 6, 7, 8]$, $K = 3$
- *result:* $[3, 2, 1, 6, 5, 4, 8, 7]$


## Approach: Chunk Traversal with Two Pointers

- the problem breaks down into applying the standard array-reversal algorithm over chunks of size $K$.

- loop through the array, incrementing the index $i$ by $K$ in each step ($i = 0, K, 2K, \dots$).
- for each chunk starting at $i$, the left pointer is $i$.
- the right pointer is the end of the chunk: $i + K - 1$. However, if this exceeds the array boundaries, cap it at $N - 1$.
- apply the standard [[Two Pointers]] reversal by swapping `A[left]` and `A[right]`, incrementing `left`, and decrementing `right` until they cross.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

void reverseInGroups(vector<int>& arr, int k) {
    int n = arr.size();
    
    for (int i = 0; i < n; i += k) {
        int left = i;
        // Ensure we don't go out of bounds for the last chunk
        int right = min(i + k - 1, n - 1);
        
        while (left < right) {
            swap(arr[left], arr[right]);
            left++;
            right--;
        }
    }
}
```

> [!tip]
> In Modern C++, you could simply use `std::reverse(arr.begin() + i, arr.begin() + min(i + k, n))` inside the loop to achieve the same result cleanly and efficiently without writing manual two-pointer swapping.

> [!warning]
> A common pitfall is forgetting that $K$ can be arbitrarily large. Using `min(i + k - 1, n - 1)` gracefully handles cases where $K > N$ by simply reversing the entire array once.


## Complexity Analysis
- **time Complexity:** $O(N)$. Despite the nested `while` loop, every element in the array is swapped exactly once. The total operations scale linearly with $N$.
- **space Complexity:** $O(1)$. The reversals are done strictly in-place.

NEXT: [[Index]]
