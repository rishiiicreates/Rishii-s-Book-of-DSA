---
type: concept
tags: [two-pointer, cpp, string, palindrome, math]
date: 2026-06-30
---
# Structural Palindrome Verification

## Problem Statement
Determine mathematically whether a given continuous string sequence is a valid structural palindrome (reads identically forwards and backwards).

---

## Approach: Symmetrical Pointer Convergence

Evaluating palindromic structure inherently depends on mathematical symmetry. Rather than allocating $\mathcal{O}(N)$ space to construct a fully reversed string in memory and executing an equality assertion, we enforce a strict memory boundary using a [[Two-Pointer]] inspection.
1. The left pointer $L$ initializes at $0$.
2. The right pointer $R$ initializes at $N - 1$.
3. Sequentially evaluate structural equivalence: `if (str[L] != str[R]) return false;`
4. Symmetrically converge $L$ upwards and $R$ downwards.
5. If the pointers intersect at the mathematical center without triggering a violation, the sequence is structurally validated.

---

## Code Implementation

```cpp
#include <string>

using namespace std;

bool isPalindrome(const string& str) {
    int L = 0;
    int R = str.length() - 1;
    
    // Evaluate symmetrical invariance
    while (L < R) {
        if (str[L] != str[R]) {
            return false; // Mathematical symmetry broken
        }
        L++;
        R--;
    }
    
    return true; // Strict sequence invariance confirmed
}
```

> [!important]
> For complex string inputs containing spaces, punctuation, or mixed capitalization, you must strictly implement alphanumeric filtering (`std::isalnum`) and uniform casing (`std::tolower`) during pointer traversal, conditionally incrementing bounds to skip structural noise.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. The string is evaluated linearly, strictly bounding operations at $\lfloor N / 2 \rfloor$ comparisons.
- **Space Complexity:** $\mathcal{O}(1)$ auxiliary bounds, evaluated strictly in-place.
