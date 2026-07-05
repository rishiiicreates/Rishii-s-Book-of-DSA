# Structural Palindrome Verification

## Problem Statement
- determine mathematically whether a given continuous string sequence is a valid structural palindrome (reads identically forwards and backwards).


## Approach: Symmetrical Pointer Convergence

- evaluating palindromic structure inherently depends on mathematical symmetry. Rather than allocating $\mathcal{O}(N)$ space to construct a fully reversed string in memory and executing an equality assertion, we enforce a strict memory boundary using a [[Two-Pointer]] inspection.
- the left pointer $L$ initializes at $0$.
- the right pointer $R$ initializes at $N - 1$.
- sequentially evaluate structural equivalence: `if (str[L] != str[R]) return false;`
- symmetrically converge $L$ upwards and $R$ downwards.
- if the pointers intersect at the mathematical center without triggering a violation, the sequence is structurally validated.


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


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. The string is evaluated linearly, strictly bounding operations at $\lfloor N / 2 \rfloor$ comparisons.
- **space Complexity:** $\mathcal{O}(1)$ auxiliary bounds, evaluated strictly in-place.

NEXT: [[Index]]
