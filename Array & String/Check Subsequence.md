---
type: concept
tags: [string, cpp, two-pointers, greedy]
date: 2026-06-30
---
# Check Subsequence

## Problem Statement
Determine if string $S$ is a subsequence of string $T$. 
A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

---

## Approach: Greedy Two-Pointer Traversal

To optimally find if $S$ is a subsequence of $T$, we use a **Greedy Two-Pointer** approach. 
The mathematical underpinning here relies on the fact that if a character $S[i]$ matches $T[j]$, the most optimal choice is to consume this match immediately. Searching for $S[i]$ further down in $T$ can strictly only reduce the available space to match the remaining characters of $S$.

Algorithm:
1. Initialize a pointer $i$ for string $S$, and pointer $j$ for string $T$.
2. Iterate while $i < |S|$ and $j < |T|$.
3. If $S[i] == T[j]$, it's a match! Increment both $i$ and $j$.
4. If they don't match, simply increment $j$ to check the next character in $T$.
5. If the loop terminates and $i == |S|$, it means every character of $S$ was successfully matched in order.

---

## Code Implementation

```cpp
#include <string>
using namespace std;

bool isSubsequence(string s, string t) {
    int n = s.length(), m = t.length();
    int i = 0, j = 0;
    
    // Traverse both strings
    while (i < n && j < m) {
        // If characters match, advance the subsequence pointer
        if (s[i] == t[j]) {
            i++;
        }
        // Always advance the main string pointer
        j++;
    }
    
    // True if we exhausted the target subsequence
    return i == n;
}
```

> [!important]
> Notice the edge cases: if $S$ is empty (`n == 0`), the loop immediately fails to enter (since $i \not< n$), and returns `i == n` (which is `0 == 0`, `true`). An empty string is mathematically a subsequence of any string!

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(|T|)$. In the worst-case scenario, we iterate completely through the string $T$. The inner comparison operates in constant time.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm strictly operates in-place with two integer pointers.
