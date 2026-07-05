# Hollow Diamond

## Problem Statement
- given an integer $N$, print a hollow diamond pattern of size $2N$ rows.
- *example for $N = 4$:*
```
********
***  ***
**    **
*      *
*      *
**    **
***  ***
********
```


## Approach: Math & Grids

- this is very similar to the [[Butterfly]] pattern, but inverted. The logic remains the same: we have $2N$ total rows. It is perfectly horizontally symmetrical, meaning the top half (rows $1$ to $N$) is an exact mirror of the bottom half (rows $N$ down to $1$).

- for any given row $i$ in the top half (from $i=1$ to $N$):
- **left Wing Stars:** The number of stars starts at $N$ and decreases to $1$. The formula is exactly $(N - i + 1)$.
- **middle Spaces:** The number of spaces starts at $0$ and grows by $2$ each time. The formula is $2 \times (i - 1)$.
- **right Wing Stars:** Matches the left wing exactly.

- we run this logic in a loop from $i=1$ to $N$. Then, we run the exact same inner logic, but the outer loop runs backwards from $i=N$ down to $1$.


## Code Implementation

```cpp
#include <iostream>
using namespace std;

void printHollowDiamond(int n) {
    // Upper Half
    for (int i = 1; i <= n; i++) {
        // 1. Left Wing Stars
        for (int j = 1; j <= (n - i + 1); j++) cout << "*";
        
        // 2. Middle Spaces
        for (int j = 1; j <= 2 * (i - 1); j++) cout << " ";
        
        // 3. Right Wing Stars
        for (int j = 1; j <= (n - i + 1); j++) cout << "*";
        
        cout << "\n";
    }
    
    // Lower Half (Mirror Image)
    for (int i = n; i >= 1; i--) {
        // 1. Left Wing Stars
        for (int j = 1; j <= (n - i + 1); j++) cout << "*";
        
        // 2. Middle Spaces
        for (int j = 1; j <= 2 * (i - 1); j++) cout << " ";
        
        // 3. Right Wing Stars
        for (int j = 1; j <= (n - i + 1); j++) cout << "*";
        
        cout << "\n";
    }
}
```


## Complexity Analysis
- **time Complexity:** $O(N^2)$. There are $2N$ rows, and for each row, we print exactly $2N$ characters (stars + spaces). Total operations = $4N^2$.
- **space Complexity:** $O(1)$. No auxiliary memory used.

NEXT: [[Index]]
