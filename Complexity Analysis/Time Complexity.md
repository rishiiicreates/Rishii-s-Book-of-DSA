---
type: concept
tags: [complexity, theory, time-complexity]
date: 2026-06-30
---
# Time Complexity

## What is Time Complexity?
Time complexity is a theoretical measure of the **amount of time an algorithm takes to run** as a function of the length of the input. Instead of measuring actual wall-clock seconds—which vary wildly based on hardware, compiler, and system load—we count the number of *elementary operations* (like assignments, comparisons, and arithmetic operations) executed by the algorithm. 

By determining how this count grows with the input size $n$, we establish the algorithm's **Order of Growth**.

---

## Asymptotic Notations
To mathematically describe time complexity, we use asymptotic notations. They describe the limiting behavior of a function when the argument tends towards a particular value or infinity.

- ==**Big-O Notation ($O$)**==: Represents the **asymptotic upper bound**. It guarantees that the time taken will not exceed a certain rate of growth.
  - Formally: $T(n) = O(f(n))$ if there exist positive constants $c$ and $n_0$ such that $T(n) \le c \cdot f(n)$ for all $n \ge n_0$.
  - Used for **worst-case** analysis.

- ==**Omega Notation ($\Omega$)**==: Represents the **asymptotic lower bound**. It guarantees that the algorithm will take *at least* this much time.
  - Formally: $T(n) = \Omega(f(n))$ if there exist positive constants $c$ and $n_0$ such that $T(n) \ge c \cdot f(n)$ for all $n \ge n_0$.
  - Used for **best-case** analysis.

- ==**Theta Notation ($\Theta$)**==: Represents the **asymptotic tight bound**. 
  - Formally: $T(n) = \Theta(f(n))$ if $T(n) = O(f(n))$ and $T(n) = \Omega(f(n))$.

> [!important]
> While Big-O is strictly an upper bound, in industry and interviews, people colloquially use Big-O to mean the tight bound ($\Theta$). When someone asks "what is the Big-O of this?", they usually want the exact order of growth, not just *any* valid upper bound (e.g., saying $O(n^2)$ for an $O(n)$ algorithm is technically true for Big-O, but practically useless).

---

## Common Time Complexities

### $O(1)$ - Constant Time
The execution time does not depend on the input size $n$.
```cpp
int getFirstElement(vector<int>& arr) {
    return arr[0]; // Always takes the same amount of time
}
```

### $O(\log n)$ - Logarithmic Time
The problem size is reduced by a constant fraction at each step. Typical in algorithms that halve the search space.
```cpp
int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

### $O(n)$ - Linear Time
The execution time grows proportionally with the input size. We visit every element a constant number of times.
```cpp
int findMax(vector<int>& arr) {
    int max_val = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        if (arr[i] > max_val) max_val = arr[i];
    }
    return max_val;
}
```

### $O(n \log n)$ - Linearithmic Time
Common in efficient sorting algorithms like Merge Sort and Heap Sort.
```cpp
// Merge Sort divides the array in half (log n levels) 
// and merges them at each level (n work per level).
// Total work: O(n log n)
```

### $O(n^2)$ - Quadratic Time
Common in algorithms with nested loops iterating over the data.
```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j+1]) {
                swap(arr[j], arr[j+1]);
            }
        }
    }
}
```

### $O(2^n)$ - Exponential Time
Often seen in naive recursive solutions for combinatorial problems.
```cpp
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

---

## Analyzing Recursive Algorithms

For recursive functions, we express the time complexity as a **Recurrence Relation**.

For example, Merge Sort:
$$T(n) = 2T(n/2) + O(n)$$

### The Master Theorem
A direct way to solve recurrences of the form:
$$T(n) = aT(n/b) + f(n)$$
where $a \ge 1$, $b > 1$, and $f(n)$ is asymptotically positive.

Let $c = \log_b a$. Compare $f(n)$ to $n^c$:
1. If $f(n) = O(n^{c-\epsilon})$ for some $\epsilon > 0$, then $T(n) = \Theta(n^c)$.
2. If $f(n) = \Theta(n^c)$, then $T(n) = \Theta(n^c \log n)$.
3. If $f(n) = \Omega(n^{c+\epsilon})$ for some $\epsilon > 0$ (and regular condition holds), then $T(n) = \Theta(f(n))$.

> [!tip] 
> Dropping Constants: When calculating time complexity, always drop constant multipliers and lower-order terms. For example, $O(3n^2 + 5n + 10)$ simplifies to $O(n^2)$. As $n \to \infty$, the highest order term dominates the growth.
