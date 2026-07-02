---
type: concept
tags: [maths, pattern, cpp, loops, visual]
date: 2026-06-30
---
# Butterfly Pattern

## Problem Statement
Given an integer $N$, print a Butterfly pattern consisting of $2N$ rows.
*Example for $N = 4$:*
```
*      *
**    **
***  ***
********
********
***  ***
**    **
*      *
```

---

## Approach: Math & Grids

Visual patterns are simply 2D grid loops governed by linear algebra.
The butterfly pattern has $2N$ total rows. It is perfectly horizontally symmetrical, meaning the top half (rows $1$ to $N$) is an exact mirror of the bottom half (rows $N$ down to $1$).

For any given row $i$ in the top half:
- **Left Wing:** It prints exactly $i$ stars.
- **Middle Spaces:** The total width is $2N$. The two wings take up $2 \times i$ characters. The remaining space is $2N - 2i = 2(N - i)$ spaces.
- **Right Wing:** It prints exactly $i$ stars.

We can run one loop from $1$ to $N$ for the top half, and then run the exact same inner logic in a loop from $N$ down to $1$ for the bottom half!

---

## Code Implementation

```cpp
#include <iostream>
using namespace std;

void printButterfly(int n) {
    // Upper Half
    for (int i = 1; i <= n; i++) {
        // 1. Left Wing
        for (int j = 1; j <= i; j++) cout << "*";
        
        // 2. Middle Spaces (2 * (N - i))
        for (int j = 1; j <= 2 * (n - i); j++) cout << " ";
        
        // 3. Right Wing
        for (int j = 1; j <= i; j++) cout << "*";
        
        cout << "\n";
    }
    
    // Lower Half (Mirror)
    for (int i = n; i >= 1; i--) {
        // 1. Left Wing
        for (int j = 1; j <= i; j++) cout << "*";
        
        // 2. Middle Spaces (2 * (N - i))
        for (int j = 1; j <= 2 * (n - i); j++) cout << " ";
        
        // 3. Right Wing
        for (int j = 1; j <= i; j++) cout << "*";
        
        cout << "\n";
    }
}
```

> [!tip] 
> Whenever you encounter a pattern problem, don't try to guess the loops. Treat it like an Excel spreadsheet. Write down the row number $i$, the number of stars, and the number of spaces on a piece of paper, and find the linear equation (e.g., $y = mx + b$) connecting $i$ to those numbers!

---

## Complexity Analysis
- **Time Complexity:** $O(N^2)$. There are $2N$ rows, and for each row, we print exactly $2N$ characters (stars + spaces). Total operations = $4N^2$.
- **Space Complexity:** $O(1)$. No auxiliary memory used.
