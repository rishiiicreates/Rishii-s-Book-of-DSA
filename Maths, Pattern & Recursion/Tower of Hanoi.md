# Tower of Hanoi

## Problem Statement
- you have three rods: **Source**, **Auxiliary**, and **Destination**. There is a tower of $N$ disks on the Source rod, stacked in decreasing order of size.

- **rules:**
- move the entire stack to the Destination rod.
- you can only move one disk at a time.
- a larger disk can *never* be placed on top of a smaller disk.


## Approach: The Power of Recursion

- this problem is extremely difficult to solve iteratively but is the absolute perfect example of "Divide and Conquer" recursion.

- to move $N$ disks from Source to Destination:
- **recursive Step 1:** Move the top $N-1$ disks from Source to the Auxiliary rod (using Destination as a temporary holder).
- **move:** Move the remaining largest disk directly from Source to Destination.
- **recursive Step 2:** Move the $N-1$ disks from Auxiliary to Destination (using Source as a temporary holder).

- *base Case:* If $N=0$, do nothing (return).


## Code Implementation

```cpp
#include <iostream>
using namespace std;

// Function prints the exact sequence of moves
void towerOfHanoi(int n, char source, char dest, char aux) {
    // Base Case
    if (n == 0) return;
    
    // Step 1: Move n-1 disks out of the way onto the Auxiliary rod
    towerOfHanoi(n - 1, source, aux, dest);
    
    // Step 2: Move the largest disk to the Destination
    cout << "Move disk " << n << " from " << source << " to " << dest << "\n";
    
    // Step 3: Move the n-1 disks from Auxiliary onto the Destination
    towerOfHanoi(n - 1, aux, dest, source);
}

int main() {
    // Solve for 3 disks from rod A to rod C, using rod B as auxiliary
    towerOfHanoi(3, 'A', 'C', 'B');
    return 0;
}
```


## Complexity Analysis

### Time Complexity: $O(2^N)$
- to calculate the total number of moves $T(N)$, we look at the recurrence relation derived from the code:
$$ T(N) = 2T(N-1) + 1 $$
- solving this equation mathematically yields:
$$ T(N) = 2^N - 1 $$
- because it requires $2^N - 1$ moves, the time complexity is **$O(2^N)$** (Exponential Time).

> [!warning]
> The Tower of Hanoi is a classic example of exponential explosion. Moving 64 disks would require $2^{64} - 1$ moves. Even if a computer did 1 billion moves per second, it would take nearly 600 years to complete!

### Space Complexity: $O(N)$
- the memory complexity is equal to the maximum depth of the recursive Call Stack. Since we only ever recurse down to $N=0$ before popping back up, the maximum depth is exactly $N$.

NEXT: [[Index]]
