---
type: concept
tags: [complexity, theory, big-omega]
date: 2026-06-30
---
# Big-Omega Notation ($\Omega$)

## Mathematical Definition
Big-Omega notation describes the **asymptotic lower bound** of an algorithm. It guarantees that an algorithm will take *at least* a certain amount of time or space, representing the absolute theoretical floor or best-case scenario.

Let $f(n)$ and $g(n)$ be functions mapping non-negative integers to real numbers.
We say that $f(n) = \Omega(g(n))$ if there exist positive constants $c$ and $n_0$ such that:
$$ 0 \le c \cdot g(n) \le f(n) \quad \text{for all } n \ge n_0 $$

### Intuition
- $f(n)$ is the exact step count of the algorithm.
- $g(n)$ is the lower bound function.
- For large enough inputs ($n \ge n_0$), the exact time will never drop below $c \cdot g(n)$. 

> [!tip]
> Big-Omega is essentially a "greater than or equal to" ($\ge$) relationship for function growth.

---

## Use Cases for Big-Omega

1. **Best-Case Analysis:**
   Evaluating an algorithm under its most ideal conditions. 
   For example, Insertion Sort on an already sorted array takes linear time. So its best-case complexity is $\Omega(n)$.

2. **Problem Lower Bounds:**
   Sometimes $\Omega$ describes the fundamental mathematical limits of a *computational problem*, regardless of the algorithm chosen.
   - Example: Any comparison-based sorting algorithm (like QuickSort or MergeSort) requires at least $\Omega(n \log n)$ comparisons to sort an arbitrary array. It is mathematically impossible to write a comparison sort that runs in $O(n)$.

---

## Example: Best Case vs Worst Case

```cpp
int linearSearch(const vector<int>& arr, int target) {
    for (size_t i = 0; i < arr.size(); ++i) {
        if (arr[i] == target) {
            return i; // Found it!
        }
    }
    return -1; // Not found
}
```

- **Best-Case ($\Omega$):** The `target` is exactly at `arr[0]`. The loop executes exactly once. Time taken is a constant. Thus, the best-case time complexity is $\Omega(1)$.
- **Worst-Case ($O$):** The target is at the end or missing. We check all $n$ elements. Upper bound is $O(n)$ (see [[Big-O]]).

Because the upper bound and lower bound do not match ($O(n) \neq \Omega(1)$), we cannot express the general runtime using [[Theta]] notation.
