# Print n to 1

## Problem Statement
- given an integer $N$, recursively print the numbers from $N$ down to $1$. You are not allowed to use loops.


## Approach: Tail Recursion

- since we start with $N$ and need to print $N$ immediately, we place the `cout` statement *before* the recursive call.
- this is known as **Tail Recursion**.

- **base Case:** If $N == 0$, return back.
- **execution:** Print $N$ right now.
- **recursive Step:** Call `printNto1(N - 1)`.


## Code Implementation

```cpp
#include <iostream>
using namespace std;

void printNto1(int n) {
    if (n == 0) {
        return; // Base Case
    }
    
    // Execute immediately
    cout << n << " ";
    
    // Recursive Call (Tail Recursion)
    printNto1(n - 1);
}

int main() {
    printNto1(5); 
    // Outputs: 5 4 3 2 1
    return 0;
}
```

> [!tip]
> **Tail Call Optimization (TCO):** Because the recursive call `printNto1(n - 1)` is the absolute *last* operation in the function, modern compilers (like GCC with `-O2`) can optimize this to use **$O(1)$ space**. It physically reuses the same stack frame instead of pushing a new one, transforming the recursion into a highly efficient iterative loop under the hood!


## Complexity Analysis
- **time Complexity:** $O(N)$. We make $N$ function calls.
- **space Complexity:** $O(N)$ technically due to the Call Stack, but $O(1)$ in practice if the compiler applies Tail Call Optimization.

NEXT: [[Index]]
