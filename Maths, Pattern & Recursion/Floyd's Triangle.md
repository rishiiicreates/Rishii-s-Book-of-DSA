---
type: concept
tags: [maths, pattern, cpp, loops, visual, floyd]
date: 2026-06-30
---
# Floyd's Triangle

## Problem Statement
Given an integer $N$, print Floyd's Triangle of $N$ rows. Floyd's triangle is a right-angled triangular array of natural numbers, used in computer science education.
*Example for $N = 4$:*
```
1
2 3
4 5 6
7 8 9 10
```

---

## Approach: Global State Variable

This pattern introduces a new concept: a **Global State Variable** (or out-of-loop variable). 
In previous patterns like [[Solid Rectangle]], the character printed was static (always `*`). Here, the character printed is a dynamic integer that increments constantly.

1. **Outer Loop:** Controls the rows from $i = 1$ to $N$.
2. **Inner Loop:** In a right-angled triangle, the $i$-th row has exactly $i$ elements. So the inner loop runs from $j = 1$ to $i$.
3. **State Variable:** We define a variable `count = 1` *outside* both loops. Inside the inner loop, we print `count`, and immediately increment it `count++`.

---

## Code Implementation

```cpp
#include <iostream>
using namespace std;

void printFloydsTriangle(int n) {
    int count = 1; // State variable maintained across all loop iterations
    
    // Outer loop for rows
    for (int i = 1; i <= n; i++) {
        
        // Inner loop bounds are dynamically tied to the row number 'i'
        for (int j = 1; j <= i; j++) {
            cout << count << " ";
            count++;
        }
        
        cout << "\n";
    }
}
```

> [!important]
> Notice how the inner loop bounds are `j <= i`. This is the fundamental equation for all Right-Angled Triangle patterns. The width of the row is mathematically bounded by its depth.

---

## Complexity Analysis
- **Time Complexity:** $O(N^2)$. The outer loop runs $N$ times. The inner loop runs $1 + 2 + 3 + \dots + N$ times. By Gauss's Formula, the total iterations are $N(N+1)/2$, which is $O(N^2)$.
- **Space Complexity:** $O(1)$. We only use a single `count` integer.
