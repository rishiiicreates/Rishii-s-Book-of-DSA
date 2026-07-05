# Lexicographically Largest Permutation (Deque)

## Problem Statement
- given a discrete integer array sequence $A$ of size $N$, construct the lexicographically absolute largest possible permutation sequence $P$ by iteratively executing exactly one operation per element in order: pop $A[i]$ and push it to either the `front` or the `back` of an initially empty structural sequence $P$.


## Approach: Greedy Topological Bounding

- to maximize a lexicographical sequence mathematically, the absolutely largest scalars must geometrically occupy the earliest (left-most) topological indices.
- since we extract elements from $A$ in strict linear sequence from index $0$ to $N-1$, and map them via boundary insertion (front/back) to $P$, the structure is fundamentally isomorphic to a Deque.

- **greedy State Machine:**
- let $D$ be a Deque representing the output permutation $P$.
- for each scalar $x \in A$:
   - if $D$ is $\Lambda$ (empty), inject $x$ anywhere.
   - we must mathematically evaluate the relative magnitude of $x$ against the topological boundary limit $\text{front}(D)$.
   - if $x \ge \text{front}(D)$, it is mathematically superior. We explicitly push $x$ to the Front of $D$, forcing it into a superior lexicographical position.
   - if $x < \text{front}(D)$, injecting it at the Front would geometrically degrade the sequence magnitude. Thus, we push $x$ to the Back of $D$.

- by induction, this greedy local optimization generates the absolute global maximum lexicographical valid permutation under the strict insertion constraints.


## Code Implementation

```cpp
#include <vector>
#include <deque>

using namespace std;

vector<int> lexicographicallyLargestPermutation(const vector<int>& A) {
    deque<int> dq;
    
    for (int x : A) {
        if (dq.empty()) {
            dq.push_back(x);
        } else {
            // Greedy Topological Constraint Evaluation
            if (x >= dq.front()) {
                dq.push_front(x); // Maximizes prefix magnitude
            } else {
                dq.push_back(x);  // Defers inferior magnitude
            }
        }
    }
    
    // Flatten Deque to discrete sequence array
    return vector<int>(dq.begin(), dq.end());
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ strict linear scan. Each element dictates exactly one $O(1)$ boundary evaluation and insertion.
- **space Complexity:** $O(N)$ auxiliary sequence bounding.

> [!important]
> **Strict Inequality Caveat:** The constraint $x \ge \text{front}(D)$ must include structural equality! If $x == \text{front}(D)$, placing it at the front preserves the maximum prefix identical chain, whereas placing it at the back fragments the equal elements topologically, resulting in a strictly inferior lexicographical matrix.

NEXT: [[Index]]
