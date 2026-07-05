# Pyramid Pattern

## Problem Statement
- given an integer $N$, print a centered pyramid (equilateral triangle) pattern of $N$ rows using asterisks.
- *example for $N = 4$:*
```
   *
  ***
 *****
*******
```


## Approach: Space and Star Equations

- this pattern requires printing empty spaces before the stars to push them to the center. For every row $i$ (from $1$ to $N$), we need to mathematically calculate two things:
- **how many spaces?**
- **how many stars?**

- let's build a mental table for $N=4$:
- | row ($i$) | Spaces | Stars |
- |---|---|---|
- | 1 | 3 | 1 |
- | 2 | 2 | 3 |
- | 3 | 1 | 5 |
- | 4 | 0 | 7 |

- **the Linear Equations:**
- **spaces:** Look at the relationship. $3, 2, 1, 0$. This perfectly matches $N - i$.
- **stars:** Look at the relationship. $1, 3, 5, 7$. This is the mathematical formula for odd numbers: $2i - 1$.

- so for any row $i$: print $(N - i)$ spaces, then print $(2i - 1)$ stars.


## Code Implementation

```cpp
#include <iostream>
using namespace std;

void printPyramid(int n) {
    for (int i = 1; i <= n; i++) {
        
        // 1. Print the leading spaces
        for (int j = 1; j <= (n - i); j++) {
            cout << " ";
        }
        
        // 2. Print the stars
        for (int j = 1; j <= (2 * i - 1); j++) {
            cout << "*";
        }
        
        // 3. Newline
        cout << "\n";
    }
}
```

> [!tip]
> You do **not** need to print the trailing spaces on the right side of the pyramid. Once the stars are printed, simply drop a `\n` to go to the next line. Printing trailing spaces wastes CPU cycles.


## Complexity Analysis
- **time Complexity:** $O(N^2)$. For each of the $N$ rows, we print exactly $(N-i) + (2i-1) = N + i - 1$ characters. The total prints scale quadratically.
- **space Complexity:** $O(1)$.

NEXT: [[Index]]
