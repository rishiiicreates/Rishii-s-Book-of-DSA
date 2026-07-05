# Next Greater Element

## Problem Statement
- given an array $A$ of size $N$, find the Next Greater Element (NGE) for every element in the array. The NGE for an element $A[i]$ is defined mathematically as the first element $A[j]$ such that $j > i$ and $A[j] > A[i]$. If no such element exists, the NGE is defined as $-1$.

- *example:* $A = [4, 5, 2, 25]$
- *result:* $[5, 25, 25, -1]$


## Approach: Monotonic Decreasing Stack

- a naive implementation utilizes nested loops yielding $O(N^2)$ time. We can optimize this by maintaining a [[Monotonic Stack]]. Specifically, we iterate from right to left (index $N-1$ down to $0$) and construct a stack that preserves a **monotonically decreasing** invariant from the bottom to the top.

- **right-to-Left Traversal:** Iterating backwards inherently grants us visibility of the future (elements to the right) as we arrive at $A[i]$.
- **preserving Monotonicity:** While the stack is not empty and the top of the stack is $\le A[i]$, pop elements from the stack.
   - *why?* Because any element $\le A[i]$ to its right can *never* be the Next Greater Element for any element to the left of $i$. $A[i]$ itself acts as a larger, closer barrier. Thus, smaller elements are mathematically obsoleted.
- **query Response:** After popping obsoleted elements, if the stack is empty, there is no element strictly greater than $A[i]$ to its right. We record $-1$. Otherwise, the top of the stack is unequivocally the NGE.
- **state Insertion:** Push the current element $A[i]$ onto the stack.

> [!important]
> For variants involving **circular arrays** (where the array wraps around), we mathematically simulate concatenation by running the loop from $2N - 1$ down to $0$, applying the exact same stack logic but using `A[i % N]` for array lookups.


## Code Implementation

```cpp
#include <vector>
#include <stack>
using namespace std;

vector<long long> nextLargerElement(const vector<long long>& arr) {
    int n = arr.size();
    vector<long long> nge(n, -1);
    stack<long long> st;
    
    // Traverse from right to left
    for (int i = n - 1; i >= 0; i--) {
        // Enforce Monotonic Decreasing Invariant
        // Elements smaller than or equal to current are useless for elements to the left
        while (!st.empty() && st.top() <= arr[i]) {
            st.pop();
        }
        
        // Record the NGE (the closest larger element)
        if (!st.empty()) {
            nge[i] = st.top();
        }
        
        // Current element is now a candidate NGE for elements to its left
        st.push(arr[i]);
    }
    
    return nge;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$. Although there is a `while` loop nested inside the `for` loop, every element $A[i]$ is pushed onto the stack exactly once and popped at most once. The total amortized cost across all iterations is strictly $O(N)$.
- **space Complexity:** $O(N)$. The monotonic stack can theoretically store up to $N$ elements in the worst case (if the array is strictly decreasing, e.g., $A = [5, 4, 3, 2, 1]$).

NEXT: [[Index]]
