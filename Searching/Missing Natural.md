# Missing Natural

## Problem Statement
- given an array containing $n$ distinct numbers taken from the range $[0, n]$, find the single number missing from the array.
- *example:* `[3, 0, 1]` returns `2`.


## Approach: Mathematical Summation vs Bitwise XOR

- while a sorting-based or hash-based approach takes $O(N \log N)$ or $O(N)$ space respectively, we can solve this in $O(N)$ time and $O(1)$ space using two rigorous mathematical techniques.

### Approach 1: Sum Formula (Gauss Summation)
- the sum of the first $n$ natural numbers is given by the formula:
$$ \Sigma = \frac{n(n + 1)}{2} $$
- since exactly one number is missing from the continuous range $[0, n]$, the missing number is strictly the mathematical difference between the theoretical sum $\Sigma$ and the actual sum of the elements in the array.

### Approach 2: Bitwise XOR
- bitwise XOR ($\oplus$) has two critical properties:
- $x \oplus X = 0$ (Self-inverse)
- $x \oplus 0 = X$ (Identity)

- if we XOR all the numbers from $0$ to $n$, and then XOR that result with all the actual elements present in the array, every number that appears in both the range and the array will cancel out. The only number left will be the missing number.


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

// Method 1: Mathematical Sum
int missingNumberMath(const vector<int>& nums) {
    int n = nums.size();
    long long expectedSum = 1LL * n * (n + 1) / 2;
    long long actualSum = 0;
    
    for (int num : nums) {
        actualSum += num;
    }
    
    return expectedSum - actualSum;
}

// Method 2: Bitwise XOR
int missingNumberXOR(const vector<int>& nums) {
    int n = nums.size();
    int xorSum = 0;
    
    for (int i = 0; i <= n; i++) {
        xorSum ^= i;
    }
    
    for (int num : nums) {
        xorSum ^= num;
    }
    
    return xorSum;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ for both approaches, as they iterate through the array once.
- **space Complexity:** $O(1)$ auxiliary space.

> [!warning]
> **Integer Overflow:** The Sum Formula is mathematically elegant but susceptible to integer overflow if $n$ is very large (e.g., $n > 10^5$, $\Sigma \approx 5 \times 10^9$). You must use 64-bit integers (`long long`). The XOR approach is immune to overflow and is practically superior.

NEXT: [[Index]]
