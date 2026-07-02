---
type: concept
tags: [maths, pattern, cpp, loops, basics, visual]
date: 2026-06-30
---
# Solid Rectangle

## Problem Statement
Given two integers $R$ (rows) and $C$ (columns), print a solid rectangle of asterisks `*` of size $R \times C$.
*Example for $R = 3, C = 5$:*
```
*****
*****
*****
```

---

## Approach: Nested Loops

The most basic pattern generation involves a nested loop structure. 
Think of the screen as a 2D matrix or a 2D Cartesian coordinate system.
1. The **Outer Loop** controls the rows ($y$-axis). It moves the cursor down line by line.
2. The **Inner Loop** controls the columns ($x$-axis). It prints characters horizontally across the current row.

For a completely solid rectangle, the inner loop always prints a `*`, and it does this $C$ times. Once the inner loop finishes, the outer loop must print a newline `\n` to drop down to the next row.

---

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

---

## Complexity Analysis
- **Time Complexity:** $O(R \times C)$. We execute the print statement exactly $R \times C$ times.
- **Space Complexity:** $O(1)$. No extra memory is allocated.
