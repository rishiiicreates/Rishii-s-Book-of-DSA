# Valid Palindrome II

## Problem Statement
- given a string $S$, mathematically verify if $S$ can form a true geometric palindrome by structurally deleting *at most* one discrete character.


## Approach: Two Pointers with Branching Tolerance

- a pure mathematical palindrome is defined by structural reflection symmetry: $S[i] = S[N - 1 - i]$ for all $i \in [0, \lfloor N/2 \rfloor]$.
- this is verified efficiently via Two Pointers converging from $L=0$ and $R=N-1$.

- when $S[L] == S[R]$, the symmetry holds, and we converge both pointers inward.
- if a divergence occurs ($S[L] \ne S[R]$), the rigid palindrome property breaks.
- to satisfy the "at most one deletion" constraint, we have exactly two hypothetical geometric branch resolutions:
- assume the topological error is at $L$. Delete $S[L]$ and verify if the sub-sequence $S[L+1 \dots R]$ is a pure palindrome.
- assume the topological error is at $R$. Delete $S[R]$ and verify if the sub-sequence $S[L \dots R-1]$ is a pure palindrome.

- if either strict branch evaluates to `true`, the condition holds. If both branches fail, the string requires at least two discrete deletions, failing the fundamental constraint.


## Code Implementation

```cpp
#include <string>

using namespace std;

// Helper function to verify strict structural symmetry
bool isStrictPalindrome(const string& s, int L, int R) {
    while (L < R) {
        if (s[L] != s[R]) return false;
        L++;
        R--;
    }
    return true;
}

bool validPalindrome(string s) {
    int L = 0;
    int R = s.length() - 1;
    
    while (L < R) {
        if (s[L] == s[R]) {
            L++;
            R--;
        } else {
            // Divergence state: Trigger bifurcated verification
            return isStrictPalindrome(s, L + 1, R) || isStrictPalindrome(s, L, R - 1);
        }
    }
    
    return true; // Zero deletions required
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$. In the absolute worst case, the main loop traverses up to the middle, and the divergence verification traverses the remainder exactly once per branch. Thus bounded by $O(N) + O(N) = O(N)$.
- **space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Greedy Validation Limits:** Why can't we simply continue skipping errors? The problem strictly defines a tolerance factor of exactly $K=1$. For arbitrary $K$ deletions, the Two Pointers model collapses into branching permutations, transitioning the problem domain to Dynamic Programming (specifically, Edit Distance to Reverse Sequence).

NEXT: [[Index]]
