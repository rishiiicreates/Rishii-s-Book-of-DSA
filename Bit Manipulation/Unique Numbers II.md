---
type: concept
tags: [bit manipulation, cpp, array]
date: 2026-06-30
---
# Unique Numbers II (Single Number II)

## Problem Statement
Given an integer array $A$ where every element appears exactly three times except for one unique element, which appears exactly once. Find that single element.

*Example:* $A = [2, 2, 3, 2]$
*Result:* $3$

---

## Approach: Bitwise State Machine (Optimal)

Since numbers appear $3$ times, a standard XOR ($\oplus$) will not cancel them out perfectly. We need to construct a mathematical base-3 counter for each bit position.

We can design a state machine that counts the occurrences of `1`s at each bit position modulo 3. The states for any bit are $0 \to 1 \to 2 \to 0$. We need two bits to represent three states:
- `ones`: Tracks bits that have appeared exactly $1$ time (modulo 3).
- `twos`: Tracks bits that have appeared exactly $2$ times (modulo 3).

For an incoming bit `x`:
1. We add `x` to `ones`. But if `x` was already in `twos`, it means this is the third occurrence, and it should roll over to $0$. So: `ones = (ones ^ x) & ~twos`.
2. We add `x` to `twos`. If `x` was just added to `ones` (meaning it's the first occurrence), it shouldn't go to `twos`. So: `twos = (twos ^ x) & ~ones`.
3. After processing all numbers, the bits of the unique number will be held in `ones`, because the unique number appears exactly once (so its bits entered `ones` and never progressed to `twos` or rolled over).

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int singleNumber(const vector<int>& nums) {
    int ones = 0;
    int twos = 0;
    
    for (int x : nums) {
        // Add to `ones` if it's not already in `twos`
        ones = (ones ^ x) & ~twos;
        
        // Add to `twos` if it's not already in `ones`
        twos = (twos ^ x) & ~ones;
    }
    
    return ones;
}
```

> [!important]
> The order of operations is vital here. You compute `ones` first, and then the newly updated `ones` is used in the mask `~ones` when computing `twos`. This cleanly implements the transition logic without needing temporary variables.

> [!tip]
> This logic naturally extends. If elements appeared $5$ times except one, you would construct a base-5 counter requiring three variables (`ones`, `twos`, `threes`) and appropriate masking.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We process the array in a single linear pass.
- **Space Complexity:** $O(1)$. Only two state variables are maintained.
