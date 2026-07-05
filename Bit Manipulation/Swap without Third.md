# Swap without Third Variable

## Problem Statement
- given two mathematical variables $A$ and $B$, swap their discrete values strictly in-place, without allocating memory for a temporary third variable.


## Approach: XOR Symmetric Difference

- we can exploit the mathematical properties of the bitwise XOR operator ($\oplus$).
- xOR forms an abelian group over the Boolean domain with the following axioms:
- **identity:** $X \oplus 0 = X$
- **self-Inverse:** $X \oplus X = 0$
- **associativity:** $(X \oplus Y) \oplus Z = X \oplus (Y \oplus Z)$

- by leveraging these axioms, we can superimpose the information of $A$ and $B$ into a single variable and subsequently extract them.

- let the initial states be $A_0$ and $B_0$.
- **step 1:** $A_1 = A_0 \oplus B_0$ (Now $A$ holds the combined state of both)
- **step 2:** $B_1 = A_1 \oplus B_0 \implies (A_0 \oplus B_0) \oplus B_0 \implies A_0 \oplus (B_0 \oplus B_0) \implies A_0 \oplus 0 \implies A_0$. (Now $B$ holds the original value of $A$).
- **step 3:** $A_2 = A_1 \oplus B_1 \implies (A_0 \oplus B_0) \oplus A_0 \implies (A_0 \oplus A_0) \oplus B_0 \implies 0 \oplus B_0 \implies B_0$. (Now $A$ holds the original value of $B$).


## Code Implementation

```cpp
#include <iostream>

using namespace std;

void swapWithoutThird(int& a, int& b) {
    // Crucial check: If a and b are referencing the exact same memory address,
    // a = a ^ a will immediately zero out the variable.
    if (&a != &b) {
        a = a ^ b; // a now holds the composite difference
        b = a ^ b; // b extracts the original a
        a = a ^ b; // a extracts the original b
    }
}
```


## Complexity Analysis
- **time Complexity:** $O(1)$. Exactly three ALU instructions.
- **space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Pointer Aliasing Hazard:** If this swap function is passed two pointers to the exact same element in an array (e.g., `swapWithoutThird(arr[i], arr[i])`), the operation $A \oplus A$ destroys the data, overwriting it with $0$. Always guard the XOR swap with `if (&a != &b)`.

NEXT: [[Index]]
