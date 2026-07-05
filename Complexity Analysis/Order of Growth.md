# Order of Growth

## What is the Order of Growth?
- the **Order of Growth** is an abstraction used in [[Asymptotic Analysis]] to classify how the resource requirements (time or space) of an algorithm scale with respect to the input size $n$.

- it describes the dominant factor in the algorithm's runtime equation. When inputs grow large, the highest order term completely overshadows the lower-order terms and constant multipliers.


## The Golden Rules
- when determining the order of growth from an exact equation, apply these two rules:

- **drop Constants:** Constant multipliers do not affect how the function *scales*.
   - example: $T(n) = 100n \implies \text{Order of Growth is } n$.
   - a function taking $100n$ operations will double its runtime if $n$ doubles, exactly like a function taking $1n$ operations.

- **drop Lower-Order Terms:** As $n \to \infty$, the highest-degree term dominates.
   - example: $T(n) = 5n^2 + 3n + 10 \implies \text{Order of Growth is } n^2$.
   - for $n = 1000$, the $n^2$ term is $1,000,000$, whereas $3n$ is just $3,000$. The lower-order terms become statistically insignificant.

> [!important]
> Order of growth classifies functions into discrete tiers. We say $O(n)$ or $O(n^2)$ to group algorithms by their fundamental scaling behavior.


## Hierarchy of Growth Rates
- from fastest (most efficient) to slowest (least efficient), here are the most common orders of growth:

- | notation | Name | Characteristic |
- | :--- | :--- | :--- |
- | $o(1)$ | Constant | Runtime is independent of input size. |
- | $o(\log n)$ | Logarithmic | Problem size is divided by a constant fraction per step. |
- | $o(n)$ | Linear | Runtime is directly proportional to input size. |
- | $o(n \log n)$| Linearithmic | Splitting into subproblems and doing linear work to merge. |
- | $o(n^2)$ | Quadratic | Nested loops over the input. |
- | $o(n^3)$ | Cubic | Three nested loops. |
- | $o(2^n)$ | Exponential | Exploring all subsets or naive branching recursions. |
- | $o(n!)$ | Factorial | Exploring all permutations. |

$$ 1 \prec \log n \prec \sqrt{n} \prec n \prec n \log n \prec n^2 \prec 2^n \prec n! $$


## Geometric Intuition
- if you plot these functions on a graph where the x-axis is $n$ and the y-axis is operations:
- $o(\log n)$ almost flattens out entirely.
- $o(n)$ is a straight diagonal line.
- $o(n^2)$ curves upwards like a parabola.
- $o(2^n)$ shoots straight up almost vertically, becoming instantly uncomputable for even small inputs like $n = 50$.

NEXT: [[Index]]
