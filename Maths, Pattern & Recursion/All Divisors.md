---
type: concept
tags: [maths, pattern, cpp, divisors]
date: 2026-06-30
---
# All Divisors

## Problem Statement
Given an integer $N$, find all its positive divisors and return them in ascending order.
*Example:* For $N = 36$, the divisors are `[1, 2, 3, 4, 6, 9, 12, 18, 36]`.

---

## Approach: The $\sqrt{N}$ Method

A naive $O(N)$ loop from $1 \dots N$ checking `if (N % i == 0)` is too slow for large constraints.

Just like in [[Prime Testing]], we leverage the mathematical fact that divisors always exist in pairs. 
If $i$ is a divisor of $N$, then $(N/i)$ must also be a divisor of $N$.
Because one divisor in the pair must be $\le \sqrt{N}$, we only need to iterate up to the square root of $N$. Every time we find a divisor $i$, we instantly find its counterpart $(N/i)$.

### The Perfect Square Trap
If $N = 36$ and $i = 6$, the counterpart is $36/6 = 6$. We must ensure we don't add the number $6$ twice!

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

vector<int> allDivisors(int n) {
    vector<int> divisors;
    
    // Iterate up to the square root
    for (int i = 1; i * i <= n; i++) {
        if (n % i == 0) {
            divisors.push_back(i);        // Add the small divisor
            
            // Check to prevent duplicate additions for perfect squares
            if (i != n / i) {
                divisors.push_back(n / i); // Add the paired large divisor
            }
        }
    }
    
    // The problem usually requires sorted output
    sort(divisors.begin(), divisors.end());
    
    return divisors;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\sqrt{N} + K \log K)$ where $K$ is the total number of divisors. Finding the divisors takes $O(\sqrt{N})$. Sorting them at the end takes $O(K \log K)$. Because $K$ is typically very small compared to $N$, this is incredibly fast.
- **Space Complexity:** $O(K)$ to store the array of divisors that is returned.

> [!important]
> If a problem strictly prohibits sorting at the end, you can maintain two separate lists: one for the small divisors (`i`) and one for the large divisors (`n / i`). The small divisors are naturally found in ascending order, while the large divisors are found in descending order. Reverse the large list and append it to the small list to achieve an un-sorted $O(\sqrt{N})$ overall time complexity!
