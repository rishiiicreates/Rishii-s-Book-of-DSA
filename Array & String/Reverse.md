# Reverse an Array

## Problem Statement
- given an array of size $N$, reverse the order of its elements in-place.
- mathematically, for every valid index $i \in [0, N-1]$, the element at index $i$ must be swapped with the element at index $N - 1 - i$.


## Approach: Two-Pointer Convergence

- the most optimal way to reverse a sequence is to use a converging **Two-Pointer** technique.
- since the mathematical mapping maps $i \to N - 1 - i$, each swap inherently places two elements into their final correct positions simultaneously.
- by maintaining a `left` pointer initialized at $0$ and a `right` pointer initialized at $N-1$:
- we swap the elements at `left` and `right`.
- we increment `left` and decrement `right` ($left \to left+1, right \to right-1$).
- the process strictly halts when `left >= right`.

- for an array of even length, the pointers will cross (`left > right`). For an array of odd length, the pointers will collide at the exact median element (`left == right`), which mathematically maps to itself and does not require swapping.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

void reverseArray(vector<int>& arr) {
    int left = 0;
    int right = arr.size() - 1;
    
    // Strict mathematical condition for convergence
    while (left < right) {
        swap(arr[left], arr[right]);
        left++;
        right--;
    }
}
```

> [!tip]
> In modern C++, this entire algorithm is encapsulated mathematically by `std::reverse(arr.begin(), arr.end())`. The STL implementation heavily utilizes iterators and compile-time type traits to achieve absolute optimal native performance.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. The loop iterates exactly $\lfloor N/2 \rfloor$ times. Since swap is an $\mathcal{O}(1)$ operation, the overall time bound scales linearly.
- **space Complexity:** $\mathcal{O}(1)$. The algorithm requires strictly two integer variables to track indices, heavily constraining the auxiliary space footprint.

NEXT: [[Index]]
