# Print 1 to n

## Problem Statement
- given an integer $N$, recursively print the numbers from $1$ to $N$. You are not allowed to use loops.


## Approach: Head Recursion

- since we start with $N$ and need to print $1$ first, we must delay the `cout` statement until *after* the recursive call returns.
- this is known as **Head Recursion**.

- **base Case:** If $N == 0$, return back.
- **recursive Step:** Call `print1toN(N - 1)`.
- **execution:** Only *after* the base case is hit and the frames start popping off the Call Stack do we execute `cout << N`. This ensures the smallest numbers print first.


## Code Implementation

```cpp
#include <iostream>
using namespace std;

void print1toN(int n) {
    if (n == 0) {
        return; // Base Case
    }
    
    // Recursive Call (Head Recursion)
    print1toN(n - 1);
    
    // This line executes AFTER the recursive call returns
    // So for print1toN(3), it waits for print1toN(2) to finish, which waits for print1toN(1)...
    cout << n << " ";
}

int main() {
    print1toN(5); 
    // Outputs: 1 2 3 4 5
    return 0;
}
```

> [!important]
> **Head vs Tail Recursion:** Because the recursive call is *not* the last statement in the function (the `cout` happens after it), the compiler cannot optimize the stack frame. It must keep all $N$ frames alive in memory simultaneously.


## Complexity Analysis
- **time Complexity:** $O(N)$. We make $N$ function calls.
- **space Complexity:** $O(N)$. Maximum depth of the Call Stack is $N$.

NEXT: [[Index]]
