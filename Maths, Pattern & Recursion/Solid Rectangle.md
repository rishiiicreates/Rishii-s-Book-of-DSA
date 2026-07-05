# Solid Rectangle

## Problem Statement
- given two integers $R$ (rows) and $C$ (columns), print a solid rectangle of asterisks `*` of size $R \times C$.
- *example for $R = 3, C = 5$:*
```
*****
*****
*****
```


## Approach: Nested Loops

- the most basic pattern generation involves a nested loop structure.
- think of the screen as a 2D [[Matrix]] or a 2D Cartesian coordinate system.
- the **Outer Loop** controls the rows ($y$-axis). It moves the cursor down line by line.
- the **Inner Loop** controls the columns ($x$-axis). It prints characters horizontally across the current row.

- for a completely solid rectangle, the inner loop always prints a `*`, and it does this $C$ times. Once the inner loop finishes, the outer loop must print a newline `\n` to drop down to the next row.


## Code Implementation

```cpp
#include <iostream>
using namespace std;

void printSolidRectangle(int r, int c) {
    // Outer loop controls the rows
    for (int i = 1; i <= r; i++) {
        
        // Inner loop controls the columns
        for (int j = 1; j <= c; j++) {
            cout << "*";
        }
        
        // Move to the next line after completing a row
        cout << "\n";
    }
}
```

> [!tip] 
> **Performance:** In C++, always use `\n` instead of `endl` when printing massive patterns. `endl` forces the OS to flush the output buffer every single line, which is extremely slow and can cause Time Limit Exceeded (TLE) errors in competitive programming.


## Complexity Analysis
- **time Complexity:** $O(R \times C)$. We execute the print statement exactly $R \times C$ times.
- **space Complexity:** $O(1)$. No extra memory is allocated.

NEXT: [[Index]]
