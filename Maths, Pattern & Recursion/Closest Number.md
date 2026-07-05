# Closest Number

## Problem Statement
- given two integers $N$ and $M$ ($M \neq 0$), find the number closest to $N$ that is perfectly divisible by $M$. If there are two equally close numbers, return the one with the maximum absolute value.

- *example:* $N = 13, M = 4$. Multiples of 4 are $8, 12, 16$. The closest to 13 is $12$.


## Approach: Mathematical Quotients

- instead of looping forwards and backwards from $N$, we can use integer division logic.
- when you divide $N$ by $M$ using integer division (`q = N / M`), you find the closest multiple of $M$ that is *less than or equal* to $N$ (e.g., $13 / 4 = 3$, and $3 \times 4 = 12$).

- the only other candidate for the closest number is the *next* multiple of $M$, which is $(q \times M) + M$ if $N \times M > 0$, or $(q \times M) - M$ otherwise.

- find the quotient $q = N / M$.
- find the floor multiple $n1 = M \times q$.
- find the next multiple $n2$. If $N \times M > 0$, $n2 = M \times (q + 1)$. Else $n2 = M \times (q - 1)$.
- compare the absolute differences $|N - n1|$ and $|N - n2|$ and return the closer one.


## Code Implementation

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int closestNumber(int n, int m) {
    int q = n / m;
    
    // Candidate 1: the floor multiple
    int n1 = m * q;
    
    // Candidate 2: the next multiple
    int n2;
    if ((n * m) > 0) { // Same sign
        n2 = m * (q + 1);
    } else {           // Different signs
        n2 = m * (q - 1);
    }
    
    // Compare distances
    if (abs(n - n1) < abs(n - n2)) {
        return n1;
    } 
    return n2; 
    // Notice if they are equal, returning n2 satisfies the "maximum absolute value" rule
}
```


## Complexity Analysis
- **time Complexity:** $O(1)$. Pure arithmetic operations.
- **space Complexity:** $O(1)$.

NEXT: [[Index]]
