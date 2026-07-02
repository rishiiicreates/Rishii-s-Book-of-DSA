---
type: concept
tags: [array, cpp, majority-element, boyer-moore]
date: 2026-07-01
---
# Majority Element

## Problem Statement
Given an array $A$ of size $N$, find the majority element. The majority element is defined as the element that appears strictly more than $\lfloor N / 2 \rfloor$ times. 
*Assumption:* The majority element is guaranteed to exist in the array.
*Example:* `[2, 2, 1, 1, 1, 2, 2]` returns `2`.

---

## Approach: Boyer-Moore Voting Algorithm

A naive approach using a hash map to count frequencies takes $O(N)$ space.
To solve this in $O(1)$ auxiliary space, we use the **Boyer-Moore Voting Algorithm**.

The mathematical intuition behind the algorithm relies on the fact that if an element occurs more than $\lfloor N / 2 \rfloor$ times, its frequency is greater than the combined frequencies of all other elements. Therefore, if we pair every instance of the majority element with a non-majority element and cancel them out, the majority element will still be the only one remaining.

1. Maintain a `candidate` and a `count` (initially $0$).
2. Iterate through each element $x$ in $A$:
   - If `count == 0`, we set `candidate = x` and `count = 1`.
   - Else if `x == candidate`, we increment `count`.
   - Else (if `x != candidate`), we decrement `count`.
3. Because the majority element exists, it will mathematically outlast all other elements in this pairing process, and the final `candidate` is guaranteed to be the majority element.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

int majorityElement(vector<int>& nums) {
    int count = 0;
    int candidate = 0;
    
    for (int num : nums) {
        // Assign a new candidate when the count drops to zero
        if (count == 0) {
            candidate = num;
        }
        
        // Adjust the count for the current candidate
        count += (num == candidate) ? 1 : -1;
    }
    
    return candidate;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ because we traverse the array exactly once.
- **Space Complexity:** $O(1)$ since we only need two integer variables (`candidate` and `count`) to maintain state.

> [!important]
> **What if the majority element is not guaranteed to exist?**
> If the problem does not guarantee existence, the Boyer-Moore algorithm will still return a `candidate`, but it might be incorrect. You must add a second $O(N)$ pass to count the occurrences of the `candidate` and verify if its frequency is strictly greater than $\lfloor N / 2 \rfloor$.
