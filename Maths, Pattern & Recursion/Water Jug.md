# Water Jug Problem

## Problem Statement
- given two jugs with capacities $X$ and $Y$ liters, and an infinite water supply, determine whether it is possible to measure exactly $Z$ liters using these two jugs.
- operations allowed:
- fill a jug to the brim.
- empty a jug entirely.
- pour water from one jug to the other until the receiving jug is full or the pouring jug is empty.


## Approach: Number Theory (Bézout's Identity)

- this sounds like a graph traversal (BFS/DFS) simulation problem where you simulate pouring water back and forth. However, simulation takes time and memory.

- this is a classic math trap. It can be solved in $O(1)$ space using pure **Number Theory**.

- according to **Bézout's identity**, any number of liters you can measure by pouring back and forth between two jugs of capacities $X$ and $Y$ will *always* be a linear combination of $X$ and $Y$:
$$ aX + bY = Z $$
- (where $a$ and $b$ are integers representing the net number of times you filled/emptied each jug).

- bézout's identity mathematically proves that $aX + bY = Z$ has an integer solution if and only if $Z$ is a multiple of the Greatest Common Divisor of $X$ and $Y$.

### The Rules
- you can measure $Z$ if and only if:
- $z \le X + Y$ (You can't hold more water than both jugs combined).
- $z \pmod{\text{GCD}(X, Y)} == 0$ (Bézout's Identity).


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


## Complexity Analysis
- **time Complexity:** $O(\log(\min(X, Y)))$. This is the time taken to compute the GCD using the Euclidean Algorithm.
- **space Complexity:** $O(1)$. No graph/queue required.

NEXT: [[Index]]
