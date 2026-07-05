# Iterative Power (Binary Exponentiation)

## Problem Statement
- given a real base $X$ and an integer exponent $Y$, compute $X^Y$ efficiently.


## Approach: Binary Decomposition of Exponent

- a linear iteration takes $O(Y)$ time. We can mathematically compress this to $O(\log Y)$ by representing the exponent $Y$ in base-2 binary form:
$$ Y = \sum_{i=0}^{\lfloor \log_2 Y \rfloor} b_i 2^i $$

- substituting into the exponentiation:
$$ X^Y = X^{\sum b_i 2^i} = \prod (X^{2^i})^{b_i} $$

- we iterate over the bits of $Y$ from LSB to MSB.
- at each step, we maintain the current base power $X^{2^i}$ by repeatedly squaring it ($X \leftarrow X^2$).
- if the current bit $b_i = 1$, we multiply our running result by the current base power.

- this fundamentally maps the multiplication sequence to the non-zero terms of $Y$'s polynomial expansion.


## Code Implementation

```cpp
#include <iostream>

using namespace std;

long long power(long long x, unsigned int y) {
    long long res = 1;
    
    while (y > 0) {
        // If the LSB is 1, accumulate the current base power
        if (y & 1) {
            res = res * x;
        }
        
        // Shift Y to process the next bit
        y >>= 1;
        
        // Square the base for the next iteration (X^(2^i))
        x = x * x;
    }
    
    return res;
}
```


## Complexity Analysis
- **time Complexity:** $O(\log Y)$. The loop iterates exactly equal to the bit-length of the exponent $Y$.
- **space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Modulo Arithmetic:** In competitive programming, $X^Y$ frequently exceeds 64-bit limits. You are often required to compute $(X^Y) \pmod P$. The algebraic property $(A \times B) \pmod P = ((A \pmod P) \times (B \pmod P)) \pmod P$ guarantees safety. Simply apply `% P` immediately after `res * x` and `x * x`.

NEXT: [[Index]]
