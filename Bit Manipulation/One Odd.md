# Find the Number Occurring an Odd Number of Times

## Problem Statement
- given an array $A$ of positive integers where all numbers occur an even number of times except for one number which occurs an odd number of times, find this unique number.

- *example:* $A = [4, 3, 2, 4, 1, 3, 2, 1, 2]$
- *result:* $2$


## Approach: XOR Cumulative Sum (Optimal)

- this problem elegantly exploits the mathematical properties of the Bitwise XOR operator ($\oplus$):
- **self-Inverse:** $X \oplus X = 0$ (Any number XORed with itself cancels out).
- **identity Element:** $X \oplus 0 = X$ (XORing with 0 leaves the number unchanged).
- **commutativity & Associativity:** The order of operations does not matter.

- if we compute the cumulative XOR sum of all elements in the array:
$$ S = A[0] \oplus A[1] \dots \oplus A[N-1] $$
- because all elements except one appear an even number of times, they will pair up and perfectly cancel out to $0$. The single element appearing an odd number of times will be left unpaired, resulting in $S = \text{target}$.


## Code Implementation

```cpp
#include <vector>
using namespace std;

int findOdd(const vector<int>& arr) {
    int res = 0;
    
    // XOR all elements together
    for (int x : arr) {
        res ^= x;
    }
    
    return res;
}
```

> [!tip]
> This technique mathematically isolates elements with odd frequencies. If there were *three* elements with odd frequencies, the result would be their XOR sum, not isolating any single one directly.


## Complexity Analysis
- **time Complexity:** $O(N)$. We iterate through the array exactly once.
- **space Complexity:** $O(1)$. We only use a single integer to maintain the XOR sum.

NEXT: [[Index]]
