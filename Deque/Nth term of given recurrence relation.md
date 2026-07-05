# Nth Term of Given Recurrence Relation

## Problem Statement
- given a discrete homogeneous linear recurrence relation of order $K$, mathematically defined as $T(N) = \sum_{i=1}^{K} c_i \cdot T(N-i)$, with $K$ initial boundary base cases, compute the absolute scalar $T(N)$ efficiently.


## Approach: Bounded History (Sliding Window Deque)

- while matrix exponentiation evaluates this universally in $O(K^3 \log N)$, for moderate $N$ parameters, dynamic programming solves this iteratively.
- however, maintaining the entire $O(N)$ history matrix is mathematically redundant; the recurrence function strictly requires only the immediate $K$ previous contiguous terms.

- we model this $K$-bounded historical memory using a geometric Deque of absolute size $K$.
- this transforms the structure into a topological sliding window:
- populate a Deque $D$ with the $K$ absolute base cases.
- to compute $T(M)$, iterate the scalars in $D$, compute the linear combination scalar $S$.
- geometrically slide the window: `pop_front` the absolutely oldest bound term, and `push_back` the newly synthesized scalar $S$.
- the topological state is perfectly maintained utilizing only $O(K)$ spatial memory.

- *(note: While a circular array or bounded vector also suffices mathematically, the Deque perfectly axiomatizes this "push new, forget oldest" temporal constraint).*


## Code Implementation (Assuming Order 3 relation for illustration)

- let the relation be structurally defined as $T(N) = 1 \cdot T(N-1) + 2 \cdot T(N-2) + 3 \cdot T(N-3)$.

```cpp
#include <deque>
#include <vector>

using namespace std;

// Compute T(N) for T(N) = 1*T(N-1) + 2*T(N-2) + 3*T(N-3)
long long computeNthTerm(int n, vector<long long> base_cases) {
    if (n < 3) return base_cases[n];
    
    deque<long long> window;
    // Map initial topological state
    for (int i = 0; i < 3; i++) {
        window.push_back(base_cases[i]);
    }
    
    for (int i = 3; i <= n; i++) {
        // Compute linear recurrence scalar
        long long next_val = 1 * window[2] + 2 * window[1] + 3 * window[0];
        
        // Execute structural sliding boundary shift
        window.pop_front();
        window.push_back(next_val);
    }
    
    return window.back();
}
```


## Complexity Analysis
- **time Complexity:** $O(N \cdot K)$. For large $N$, matrix exponentiation is mandatory. For small $N$, the strictly linear topological bound dominates.
- **space Complexity:** $O(K)$ fixed spatial limitation, absolutely independent of generic bounds $N$.

> [!tip]
> **Generalization Theorem:** Any sequence bounded strictly by finite historical depth $K$ can be topologically flattened to $O(K)$ space using a sliding Deque constraint, collapsing $O(N)$ dimensional Dynamic Programming geometry into a trivial memory ring.

NEXT: [[Index]]
