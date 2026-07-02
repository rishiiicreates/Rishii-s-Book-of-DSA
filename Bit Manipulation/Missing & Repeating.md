---
type: concept
tags: [bit manipulation, cpp, array]
date: 2026-06-30
---
# Missing and Repeating Number

## Problem Statement
Given an unsorted array $A$ of size $N$ with elements intended to be from $1$ to $N$, where exactly one element $Y$ is repeated and exactly one element $X$ is missing, find both $X$ and $Y$.

*Example:* $A = [3, 1, 2, 5, 3]$
*Result:* Repeating: $3$, Missing: $4$

---

## Approach: XOR Partitioning (Optimal)

This is mathematically identical to finding two distinct odd-occurring numbers. 

1. **Combined XOR:** Let $S$ be the XOR sum of all elements in $A$ *and* all integers from $1$ to $N$.
   $$ S = (A[0] \oplus \dots \oplus A[N-1]) \oplus (1 \oplus 2 \dots \oplus N) $$
   Since $X$ is missing in $A$, it appears only once in the sequence $1 \dots N$. Since $Y$ is repeated in $A$, it appears three times in total (twice in $A$, once in $1 \dots N$). All other valid numbers appear exactly twice. Thus, $S = X \oplus Y$.
2. **Bit Isolation:** Isolate the rightmost set bit of $S$ using `mask = S & -S`. This bit represents a position where $X$ and $Y$ differ.
3. **Partition and Resolve:** Iterate through both the array $A$ and the sequence $1 \dots N$. Partition all these numbers into two sets based on the isolated bit.
   - XORing all elements falling into Set 1 yields one of the targets (either $X$ or $Y$).
   - XORing all elements falling into Set 2 yields the other target.
4. **Distinguish:** Iterate through $A$ to check which of the two isolated numbers actually exists in the array. That one is $Y$ (the repeating number). The other is $X$ (the missing number).

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> findMissingAndRepeating(const vector<int>& arr) {
    int n = arr.size();
    int xorSum = 0;
    
    // 1. XOR all array elements and numbers from 1 to N
    for (int i = 0; i < n; i++) {
        xorSum ^= arr[i];
        xorSum ^= (i + 1);
    }
    
    // 2. Isolate the rightmost set bit
    unsigned int rightmostSetBit = xorSum & -(unsigned int)xorSum;
    
    int group1 = 0;
    int group2 = 0;
    
    // 3. Partition array elements and 1..N into two groups
    for (int i = 0; i < n; i++) {
        // Group array elements
        if (arr[i] & rightmostSetBit) group1 ^= arr[i];
        else group2 ^= arr[i];
        
        // Group sequence elements
        if ((i + 1) & rightmostSetBit) group1 ^= (i + 1);
        else group2 ^= (i + 1);
    }
    
    // 4. Distinguish missing from repeating
    for (int x : arr) {
        if (x == group1) {
            return {group1, group2}; // {repeating, missing}
        }
    }
    
    return {group2, group1};
}
```

> [!warning]
> The same problem can be solved mathematically using sums: $\sum A[i] - \sum i$ gives $Y - X$, and $\sum A[i]^2 - \sum i^2$ gives $Y^2 - X^2$. However, squaring numbers up to $10^5$ causes massive integer overflow. The bitwise approach is strictly safer and avoids overflow entirely.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We make sequential linear passes.
- **Space Complexity:** $O(1)$. Purely in-place scalar variables.
