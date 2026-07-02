---
type: concept
tags: [complexity, theory, big-o]
date: 2026-06-30
---
# Big-O Notation ($O$)

## Mathematical Definition
Big-O notation describes the **asymptotic upper bound** of an algorithm. It guarantees that an algorithm's runtime will not grow faster than a given function, up to a constant multiplier, for sufficiently large inputs.

Let $f(n)$ and $g(n)$ be functions mapping non-negative integers to real numbers.
We say that $f(n) = O(g(n))$ if there exist positive constants $c$ and $n_0$ such that:
$$ 0 \le f(n) \le c \cdot g(n) \quad \text{for all } n \ge n_0 $$

### Intuition
- $f(n)$ is the exact step count of the algorithm.
- $g(n)$ is the upper bound function (e.g., $n, n^2$).
- No matter how much the input grows, $f(n)$ will always remain underneath the curve of $c \cdot g(n)$ after a certain point $n_0$.

> [!tip] 
> Big-O is essentially a "less than or equal to" ($\le$) relationship for function growth. If an algorithm is $O(n)$, it is technically also $O(n^2)$ and $O(n^3)$, though we usually state the *tightest* valid upper bound.

---

## Why Big-O is the Standard
In industry, Big-O is the standard notation used in interviews and system design because we care most about the **worst-case scenario**. 
- We want to know: *“What is the maximum amount of time/memory my server will need to process this request when things go completely wrong?”*

---

## Calculating Big-O

When analyzing code, we look for the most expensive operations.

```cpp
void printPairs(vector<int>& arr) {
    int n = arr.size();
    
    // O(1) assignment
    int count = 0; 
    
    // Outer loop runs n times
    for (int i = 0; i < n; i++) {
        // Inner loop runs n times for each outer iteration
        for (int j = 0; j < n; j++) {
            cout << arr[i] << ", " << arr[j] << endl; // O(1) work
            count++;
        }
    }
}
```
**Analysis:**
- Total operations $= 1 + n \times n \times 2 = 2n^2 + 1$.
- Dropping constants and lower-order terms gives an [[Order of Growth]] of $n^2$.
- The time complexity is exactly bounded above by $O(n^2)$.
