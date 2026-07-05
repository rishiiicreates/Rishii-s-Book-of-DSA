# Hollow Rectangle

## Problem Statement
- given two integers $R$ (rows) and $C$ (columns), print a hollow rectangle of size $R \times C$. The borders should be asterisks `*` and the inside should be spaces ` `.
- *example for $R = 4, C = 5$:*
```
*****
*   *
*   *
*****
```


## Approach: Coordinate Boundary Checking

- just like [[Solid Rectangle]], we use nested loops to traverse the 2D grid. However, we don't print a `*` everywhere.

- we map the loops to Cartesian coordinates $(i, j)$ where $i$ is the row and $j$ is the column. We only print a `*` if the coordinate lies on the mathematical **Boundary** of the rectangle.

- a coordinate $(i, j)$ is on the boundary if:
- it is the first row ($i == 1$)
- it is the last row ($i == R$)
- it is the first column ($j == 1$)
- it is the last column ($j == C$)

- if *any* of those four conditions are true, print `*`. Otherwise, print a space ` `.


## Code Implementation

```cpp
#include <iostream>
using namespace std;

void printHollowRectangle(int r, int c) {
    for (int i = 1; i <= r; i++) {
        for (int j = 1; j <= c; j++) {
            
            // Boundary condition check
            if (i == 1 || i == r || j == 1 || j == c) {
                cout << "*";
            } else {
                cout << " ";
            }
            
        }
        cout << "\n";
    }
}
```


## Complexity Analysis
- **time Complexity:** $O(R \times C)$. We still have to evaluate the `if` condition and print exactly $R \times C$ characters.
- **space Complexity:** $O(1)$. No auxiliary memory used.

NEXT: [[Index]]
