---
type: concept
tags: [array, cpp, majority-element, boyer-moore]
date: 2026-07-01
---
# Majority Element II

## Problem Statement
Given an array $A$ of size $N$, find all elements that appear strictly more than $\lfloor N/3 \rfloor$ times.
*Example:* `[3, 2, 3]` returns `[3]`. `[1, 2]` returns `[1, 2]`.

---

## Approach: Extended Boyer-Moore Voting Algorithm

Mathematically, there can be at most two elements that appear strictly more than $\lfloor N/3 \rfloor$ times. (Since $2 \times (\lfloor N/3 \rfloor + 1) \le N$, a third element is impossible).

We can extend the **Boyer-Moore Voting Algorithm** to track two potential candidates.
Instead of cancelling pairs of different elements, we cancel **triplets** of mutually distinct elements. If we continuously remove triplets of three distinct elements, whatever elements survive the cancellation process are our potential majority candidates.

1. Maintain `candidate1`, `candidate2` and their respective counts `count1`, `count2` (initially $0$).
2. Traverse each element $x$ in $A$:
   - If $x$ matches `candidate1`, increment `count1`.
   - Else if $x$ matches `candidate2`, increment `count2`.
   - Else if `count1 == 0`, set `candidate1 = x` and `count1 = 1`.
   - Else if `count2 == 0`, set `candidate2 = x` and `count2 = 1`.
   - Else, $x$ is distinct from both candidates, and both candidate counts are non-zero. This forms a triplet! We cancel them out by decrementing both `count1` and `count2`.
3. Since we aren't guaranteed that these elements exceed the $\lfloor N/3 \rfloor$ threshold, we **must** perform a second pass to count the actual frequencies of `candidate1` and `candidate2` and verify them.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> majorityElement(vector<int>& nums) {
    int count1 = 0, count2 = 0;
    
    // Initialize candidates to distinct values that won't interfere
    // Technically, any values work because they will be overwritten when count is 0
    int candidate1 = 0, candidate2 = 1; 
    
    // First Pass: Find potential candidates
    for (int num : nums) {
        if (num == candidate1) {
            count1++;
        } else if (num == candidate2) {
            count2++;
        } else if (count1 == 0) {
            candidate1 = num;
            count1 = 1;
        } else if (count2 == 0) {
            candidate2 = num;
            count2 = 1;
        } else {
            // Found a triplet of distinct elements (candidate1, candidate2, num)
            count1--;
            count2--;
        }
    }
    
    // Second Pass: Verify candidates
    count1 = 0;
    count2 = 0;
    for (int num : nums) {
        if (num == candidate1) count1++;
        else if (num == candidate2) count2++;
    }
    
    vector<int> result;
    int n = nums.size();
    if (count1 > n / 3) result.push_back(candidate1);
    if (count2 > n / 3) result.push_back(candidate2);
    
    return result;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ for the first pass and $O(N)$ for the second pass, resulting in overall $O(N)$ time.
- **Space Complexity:** $O(1)$ auxiliary space as we track only a few state variables.

> [!warning]
> The exact order of `if-else` branches in the first pass is extremely critical. You **must** check if the current element matches the candidates *before* checking if the counts are zero. If a count is zero but the element matches the *other* candidate, assigning it to the zero-count candidate will lead to tracking the exact same element twice!
