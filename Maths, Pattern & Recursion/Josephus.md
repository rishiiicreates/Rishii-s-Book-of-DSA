---
type: concept
tags: [maths, pattern, recursion, cpp, josephus]
date: 2026-06-30
---
# Josephus Problem

## Problem Statement
There are $N$ people standing in a circle waiting to be executed. The counting begins at person 1 and proceeds around the circle. In each step, $K-1$ people are skipped and the $K$-th person is executed. 
Find the position of the last remaining person.

---

## Approach 1: Array Simulation
You can simulate the process using a `std::vector`. You continually calculate the index of the person to die:
`index = (index + k - 1) % array.size();`
You erase that person from the array and repeat until the array size is 1.
**Complexity:** $O(N^2)$ because `std::vector::erase` takes $O(N)$ time, and you do it $N$ times. Too slow.

---

## Approach 2: Recursive Math Mapping (Optimal)

This is a legendary application of math in recursion. 
If we know the position of the survivor among $N-1$ people, can we map it back to the original circle of $N$ people?

Yes! When the first person dies (the $K$-th person), the circle shrinks to $N-1$ people. The counting restarts from the next person (who becomes the "new" 0th person in a 0-indexed system).
To map the survivor's position in the $(N-1)$ circle back to the $N$ circle, we simply shift it forward by $K$ and wrap around using modulo $N$.

### The Recurrence Relation (0-indexed)
$$ J(N, K) = (J(N - 1, K) + K) \pmod N $$

*Base Case:* If there is only 1 person left ($N=1$), they are sitting at index 0.

---

## Code Implementation

```cpp
#include <iostream>
using namespace std;

// Returns the 0-indexed position of the survivor
int josephusZeroIndexed(int n, int k) {
    if (n == 1) {
        return 0; // Base Case: 1 person left, they are at index 0
    }
    
    // Recursive shift
    return (josephusZeroIndexed(n - 1, k) + k) % n;
}

int josephus(int n, int k) {
    // We add 1 at the very end to convert it back to 1-indexed (human readable)
    return josephusZeroIndexed(n, k) + 1;
}

int main() {
    int n = 5, k = 2;
    cout << "Survivor position: " << josephus(n, k) << "\n";
    return 0;
}
```

> [!important]
> **Why 0-Index?** Modulo arithmetic natively wraps around to 0. If you try to write the recursive function using 1-indexed math, the modulo operation will map the $N$-th person to position 0 instead of $N$, breaking the logic. **Always solve cyclic math problems using 0-indexing**, and just add 1 at the very end!

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We make exactly $N$ recursive calls, doing $O(1)$ math in each.
- **Space Complexity:** $O(N)$ due to the Call Stack depth.
