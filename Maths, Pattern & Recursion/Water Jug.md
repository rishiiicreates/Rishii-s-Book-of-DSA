---
type: concept
tags: [maths, cpp, number-theory]
date: 2026-06-30
---
# Water Jug Problem

## Problem Statement
Given two jugs with capacities $X$ and $Y$ liters, and an infinite water supply, determine whether it is possible to measure exactly $Z$ liters using these two jugs.
Operations allowed:
1. Fill a jug to the brim.
2. Empty a jug entirely.
3. Pour water from one jug to the other until the receiving jug is full or the pouring jug is empty.

---

## Approach: Number Theory (Bézout's Identity)

This sounds like a graph traversal (BFS/DFS) simulation problem where you simulate pouring water back and forth. However, simulation takes time and memory.

This is a classic math trap. It can be solved in $O(1)$ space using pure **Number Theory**.

According to **Bézout's identity**, any number of liters you can measure by pouring back and forth between two jugs of capacities $X$ and $Y$ will *always* be a linear combination of $X$ and $Y$:
$$ aX + bY = Z $$
(where $a$ and $b$ are integers representing the net number of times you filled/emptied each jug).

Bézout's identity mathematically proves that $aX + bY = Z$ has an integer solution if and only if $Z$ is a multiple of the Greatest Common Divisor of $X$ and $Y$.

### The Rules
You can measure $Z$ if and only if:
1. $Z \le X + Y$ (You can't hold more water than both jugs combined).
2. $Z \pmod{\text{GCD}(X, Y)} == 0$ (Bézout's Identity).

---

## Code Implementation

```cpp
#include <iostream>
#include <numeric> // For std::gcd
using namespace std;

bool canMeasureWater(int x, int y, int z) {
    // 1. Capacity check
    if (x + y < z) {
        return false;
    }
    
    // 2. Trivial exact match checks (optional optimization)
    if (x == z || y == z || x + y == z) {
        return true;
    }
    
    // 3. Bézout's Identity check
    return z % std::gcd(x, y) == 0;
}
```

> [!important]
> If you encounter this in an interview, do not immediately write the 3-line math solution. Interviewers often want to see you write the BFS simulation using a `Queue<pair<int,int>>` and a `HashSet` for visited states to prove you know graph traversal, before you "discover" the math optimization.

---

## Complexity Analysis
- **Time Complexity:** $O(\log(\min(X, Y)))$. This is the time taken to compute the GCD using the Euclidean Algorithm.
- **Space Complexity:** $O(1)$. No graph/queue required.
