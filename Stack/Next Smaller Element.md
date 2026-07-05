# Next Smaller Element

## Problem Statement
- given an array $A$ of size $N$, find the Next Smaller Element (NSE) for every element in the array. The NSE for an element $A[i]$ is defined mathematically as the first element $A[j]$ such that $j > i$ and $A[j] < A[i]$. If no such element exists, the NSE is conventionally set to $-1$.

- *example:* $A = [4, 8, 5, 2, 25]$
- *result:* $[2, 5, 2, -1, -1]$


## Approach: Monotonic Increasing Stack

- this is the dual property of the [[Next Greater Element]] problem. We resolve it in identical $O(N)$ time complexity by utilizing a [[Monotonic Stack]] that enforces a strictly **increasing** constraint.

- **right-to-Left Traversal:** Traverse from index $N-1$ down to $0$. We process the "future" before evaluating the current element $A[i]$.
- **preserving Monotonicity:** While the stack is not empty and the top of the stack is $\ge A[i]$, pop elements.
   - *why?* If an element on the stack is greater than or equal to $A[i]$, it is completely shadowed by $A[i]$. For any element to the left of $i$, if it searches for a smaller element to its right, it will always encounter $A[i]$ before it encounters the larger elements previously on the stack. Thus, those larger elements are mathematically obsolete.
- **query Response:** After enforcement, the stack either contains the NSE at its top, or it is empty (implying no such element exists, returning $-1$).
- **state Insertion:** Push the current element $A[i]$ onto the stack.

> [!important]
> If a problem requires returning the **index** of the Next Smaller Element rather than the scalar value (common in algorithms like finding the largest rectangle in a histogram), you simply push array indices $i$ onto the stack instead of values $A[i]$, executing comparisons via `A[st.top()]`.


## Code Implementation

```cpp
#include <vector>
#include <stack>
using namespace std;

vector<long long> nextSmallerElement(const vector<long long>& arr) {
    int n = arr.size();
    vector<long long> nse(n, -1);
    stack<long long> st;
    
    // Traverse from right to left
    for (int i = n - 1; i >= 0; i--) {
        // Enforce Monotonic Increasing Invariant
        // Obsolete larger elements because current A[i] is a superior (smaller and closer) candidate
        while (!st.empty() && st.top() >= arr[i]) {
            st.pop();
        }
        
        // Record the NSE
        if (!st.empty()) {
            nse[i] = st.top();
        }
        
        // Push current element as a candidate for elements to its left
        st.push(arr[i]);
    }
    
    return nse;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$. Irrespective of the nested `while` loop, each element from the array is subjected to exactly one push operation and at most one pop operation. The mathematical sum of all operations scales strictly linearly.
- **space Complexity:** $O(N)$. The monotonic stack absorbs $O(N)$ dynamic memory auxiliary space in the worst-case configuration (an array strictly sorted in ascending order).

NEXT: [[Index]]
