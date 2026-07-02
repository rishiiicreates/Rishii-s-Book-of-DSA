---
type: concept
tags: [array, string, cpp, palindrome, math]
date: 2026-07-01
---
# Next Smallest Palindrome

## Problem Statement
Given a positive integer represented as an array of digits, mathematically determine the *strictly next smallest palindrome* greater than the given number. The output should also be an array of digits.
*Example:* `[8, 3, 4, 2, 2, 4, 6, 9]` $\rightarrow$ `[8, 3, 4, 3, 3, 4, 3, 8]`.

---

## Approach: Left-to-Right Mirroring & Carry Propagation

A palindrome is strictly symmetric. Therefore, the most significant (leftmost) digits dictate the numerical magnitude. To form a palindrome, we must mirror the left half onto the right half. 

Let the number be represented by array $A$ of length $N$.
1. **Mirroring:** Copy the left half of $A$ and overwrite the right half. 
2. **Comparison:** 
   - If the mirrored number is strictly greater than the original number, we are done. This is the optimal minimum increment.
   - If the mirrored number is smaller than or equal to the original number, this implies mirroring wasn't enough. We must mathematically increment the numerical value of the palindrome from its center (to keep the change as small as possible).
3. **Center Increment:** Increment the middle digit(s) by 1. If this causes a carry (i.e., $9 + 1 = 10$), propagate the carry towards the left boundary.
4. **Re-mirroring:** Since the left half was modified, mirror the newly updated left half to the right half again.

**Edge Case:** If all digits are `9` (e.g., `999`), incrementing the center propagates the carry all the way out, expanding the number's length. The answer will be `1` followed by $N-1$ zeros, ending with a `1` (e.g., `1001`).

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> generateNextPalindrome(vector<int> num) {
    int n = num.size();
    
    // Edge case: All 9s
    bool allNines = true;
    for (int x : num) {
        if (x != 9) {
            allNines = false;
            break;
        }
    }
    
    if (allNines) {
        vector<int> res(n + 1, 0);
        res[0] = 1;
        res[n] = 1;
        return res;
    }
    
    vector<int> res = num;
    int mid = n / 2;
    int i = mid - 1;
    int j = (n % 2 == 0) ? mid : mid + 1;
    
    // Find the first digit that breaks symmetry
    while (i >= 0 && res[i] == res[j]) {
        i--;
        j++;
    }
    
    // Determine if mirroring the left will make the number larger
    bool leftSmaller = (i < 0 || res[i] < res[j]);
    
    // Step 1: Mirror left to right
    i = mid - 1;
    j = (n % 2 == 0) ? mid : mid + 1;
    while (i >= 0) {
        res[j++] = res[i--];
    }
    
    // Step 2 & 3: If mirroring wasn't enough, increment the middle
    if (leftSmaller) {
        int carry = 1;
        i = mid - 1;
        
        // Handle the absolute center for odd length
        if (n % 2 == 1) {
            res[mid] += carry;
            carry = res[mid] / 10;
            res[mid] %= 10;
            j = mid + 1;
        } else {
            j = mid;
        }
        
        // Propagate carry leftward and mirror immediately to the right
        while (i >= 0) {
            res[i] += carry;
            carry = res[i] / 10;
            res[i] %= 10;
            res[j++] = res[i--]; // Re-mirroring
        }
    }
    
    return res;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ since finding the asymmetry, copying the mirror, and propagating the carry all take linear passes.
- **Space Complexity:** $O(N)$ to store the resulting palindrome array.

> [!tip]
> The pointer logic `i = mid - 1` and `j = (n % 2 == 0) ? mid : mid + 1` beautifully standardizes the center logic for both odd and even length arrays.
