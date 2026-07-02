---
type: concept
tags: [hashing, cpp, math, finite-state-machine]
date: 2026-07-01
---
# Roman to Integer

## Problem Statement
Given a string $S$ representing a valid Roman numeral, transform it into its base-10 mathematical integer equivalent.

---

## Approach: Lookahead Subtraction Principle

Roman numerals operate fundamentally as an additive sequence of literal symbols, with a rigorous subtractive caveat applied to specific consecutive pairs to avoid four identical successive characters.

We project the Roman alphabet into a static Hash Map defining the atomic scalar value of each symbol.
Let $V(c)$ be the mapped integer value of character $c$.
For a sequence $S = s_0 s_1 \dots s_{n-1}$, we compute the scalar sum sequentially.

**The Lookahead Paradigm:**
At index $i$, we evaluate $V(s_i)$ and compare it structurally to the immediate successor $V(s_{i+1})$.
- **Additive State:** If $V(s_i) \ge V(s_{i+1})$, the sequence is monotonically non-increasing. $V(s_i)$ is added to the total accumulator.
- **Subtractive State:** If $V(s_i) < V(s_{i+1})$, a strict inversion occurs (e.g., $IV$, $IX$, $CM$). The symbol $s_i$ acts as a subtractive prefix. Thus, we subtract $V(s_i)$ from the accumulator.

---

## Code Implementation

```cpp
#include <string>
#include <unordered_map>

using namespace std;

int romanToInt(string s) {
    // Static mapping of the Roman atomic alphabet
    unordered_map<char, int> roman_map = {
        {'I', 1},   {'V', 5},   {'X', 10},  {'L', 50},
        {'C', 100}, {'D', 500}, {'M', 1000}
    };
    
    int accumulator = 0;
    int n = s.length();
    
    // Evaluate the sequence with 1-step lookahead
    for (int i = 0; i < n; i++) {
        if (i + 1 < n && roman_map[s[i]] < roman_map[s[i+1]]) {
            // Subtractive inversion
            accumulator -= roman_map[s[i]];
        } else {
            // Additive monotonicity
            accumulator += roman_map[s[i]];
        }
    }
    
    return accumulator;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$, where $N$ is the string length. Since maximum Roman numeral strings are fundamentally constrained in length (e.g., 3999 = `MMMCMXCIX`), this resolves effectively in $O(1)$ constant time operations.
- **Space Complexity:** $O(1)$ auxiliary space, as the hash map cardinality is strictly bounded to the 7 atomic symbols.

> [!tip]
> **Switch Optimization:** While `std::unordered_map` is academically standard, constructing the map involves heap allocation and hashing overhead. For strict performance, a `switch` statement or a static flat array `int map[256]` indexed by the `char` ASCII value yields significant micro-optimizations, bypassing hash collisions entirely.
