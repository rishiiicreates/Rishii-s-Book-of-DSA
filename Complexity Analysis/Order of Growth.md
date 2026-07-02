---
type: concept
tags: [complexity, theory, order-of-growth]
date: 2026-06-30
---
# Order of Growth

## What is the Order of Growth?
The **Order of Growth** is an abstraction used in [[Asymptotic Analysis]] to classify how the resource requirements (time or space) of an algorithm scale with respect to the input size $n$. 

It describes the dominant factor in the algorithm's runtime equation. When inputs grow large, the highest order term completely overshadows the lower-order terms and constant multipliers.

---

## The Golden Rules
When determining the order of growth from an exact equation, apply these two rules:

1. **Drop Constants:** Constant multipliers do not affect how the function *scales*. 
   - Example: $T(n) = 100n \implies \text{Order of Growth is } n$. 
   - A function taking $100n$ operations will double its runtime if $n$ doubles, exactly like a function taking $1n$ operations.

2. **Drop Lower-Order Terms:** As $n \to \infty$, the highest-degree term dominates.
   - Example: $T(n) = 5n^2 + 3n + 10 \implies \text{Order of Growth is } n^2$.
   - For $n = 1000$, the $n^2$ term is $1,000,000$, whereas $3n$ is just $3,000$. The lower-order terms become statistically insignificant.

> [!important]
> Order of growth classifies functions into discrete tiers. We say $O(n)$ or $O(n^2)$ to group algorithms by their fundamental scaling behavior.

---

## Hierarchy of Growth Rates
From fastest (most efficient) to slowest (least efficient), here are the most common orders of growth:

| Notation | Name | Characteristic |
| :--- | :--- | :--- |
| $O(1)$ | Constant | Runtime is independent of input size. |
| $O(\log n)$ | Logarithmic | Problem size is divided by a constant fraction per step. |
| $O(n)$ | Linear | Runtime is directly proportional to input size. |
| $O(n \log n)$| Linearithmic | Splitting into subproblems and doing linear work to merge. |
| $O(n^2)$ | Quadratic | Nested loops over the input. |
| $O(n^3)$ | Cubic | Three nested loops. |
| $O(2^n)$ | Exponential | Exploring all subsets or naive branching recursions. |
| $O(n!)$ | Factorial | Exploring all permutations. |

$$ 1 \prec \log n \prec \sqrt{n} \prec n \prec n \log n \prec n^2 \prec 2^n \prec n! $$

---

## Geometric Intuition
If you plot these functions on a graph where the x-axis is $n$ and the y-axis is operations:
- $O(\log n)$ almost flattens out entirely.
- $O(n)$ is a straight diagonal line.
- $O(n^2)$ curves upwards like a parabola.
- $O(2^n)$ shoots straight up almost vertically, becoming instantly uncomputable for even small inputs like $n = 50$.
