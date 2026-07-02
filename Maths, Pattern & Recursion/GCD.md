---
type: concept
tags: [maths, recursion, cpp, gcd]
date: 2026-06-30
---
# Greatest Common Divisor (GCD)

## Problem Statement
Find the Greatest Common Divisor (GCD) of two non-negative integers $A$ and $B$. The GCD is the largest positive integer that divides both $A$ and $B$ without leaving a remainder.

---

## Approach: The Euclidean Algorithm

The naive approach is to iterate from $\min(A, B)$ down to 1 and check if the current number divides both. That takes $O(\min(A, B))$ time, which is too slow.

The **Euclidean Algorithm** is a mathematical masterclass that drastically reduces the search space based on this principle:
$$ \text{GCD}(A, B) = \text{GCD}(B, A \pmod B) $$
The algorithm stops when the remainder becomes 0. At that point, the non-zero number is the GCD.

### Example Walkthrough
Find $\text{GCD}(48, 18)$:
1. $\text{GCD}(48, 18) \implies 48 \pmod{18} = 12 \implies$ Next step: $\text{GCD}(18, 12)$
2. $\text{GCD}(18, 12) \implies 18 \pmod{12} = 6 \implies$ Next step: $\text{GCD}(12, 6)$
3. $\text{GCD}(12, 6) \implies 12 \pmod 6 = 0 \implies$ Next step: $\text{GCD}(6, 0)$
4. Base case hit ($B = 0$). Answer is $6$.

---

## Code Implementation

```cpp
#include <iostream>
using namespace std;

// Recursive Euclidean Algorithm
int gcd(int a, int b) {
    if (b == 0) return a;       // Base Case
    return gcd(b, a % b);       // Recursive Step
}

// Iterative (Uses O(1) Space)
int gcdIterative(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

> [!tip] 
> **Built-in STL Function:** In modern C++ (C++17 and later), you don't need to write this yourself. Just include `<numeric>` and use `std::gcd(a, b)`.

---

## Complexity Analysis
- **Time Complexity:** $O(\log(\min(A, B)))$. The value of the modulo operation decreases by at least half every two steps. This logarithmic behavior makes it insanely fast even for $10^{18}$ numbers.
- **Space Complexity:** $O(\log(\min(A, B)))$ for the recursive Call Stack, or $O(1)$ if implemented iteratively.

## Related Concepts
The Least Common Multiple (LCM) is directly tied to the GCD by the formula:
$$ \text{LCM}(A, B) = \frac{A \times B}{\text{GCD}(A, B)} $$
*(Watch out for overflow when doing $A \times B$! Always compute it as `(A / GCD) * B` instead).*
