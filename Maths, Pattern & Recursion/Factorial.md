# Factorial

## Problem Statement
- given a non-negative integer $N$, compute its factorial, denoted as $N!$.
- the factorial of $N$ is the product of all positive integers less than or equal to $N$.
- mathematically:
$$ N! = N \times (N-1) \times (N-2) \times \dots \times 1 $$
- *note: By definition, $0! = 1$.*


## Recursive Approach

- the problem has a perfectly overlapping sub-structure. We can define $N!$ in terms of $(N-1)!$:
$$ N! = N \times (N-1)! $$

- this naturally translates into a recursive function:
- **base Case:** If $N = 0$, return 1.
- **recursive Step:** Return $N \times \text{factorial}(N-1)$.

### Code Implementation

```cpp
#include <iostream>
using namespace std;

// We use long long because factorials grow explosively fast
long long factorial(int n) {
    if (n == 0) return 1;           // Base Case
    return n * factorial(n - 1);    // Recursive Step
}

int main() {
    cout << factorial(5) << "\n"; // Outputs: 120
    return 0;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$. The function calls itself exactly $N$ times, doing $O(1)$ multiplication work at each step.
- **space Complexity:** $O(N)$. Due to the recursive calls, there will be $N$ frames pushed onto the Call Stack simultaneously before the base case is reached.

> [!tip] 
> In production environments, calculating factorial iteratively (with a `for` loop) is vastly superior because it achieves $O(1)$ space complexity by avoiding the Call Stack overhead entirely.


## The Danger of Integer Overflow

- factorials exhibit **Exponential Growth**.
- a standard 32-bit `int` can only store up to roughly $2 \times 10^9$. It overflows at just **$13!$**.
- a 64-bit `long long` can only store up to roughly $9 \times 10^{18}$. It overflows at just **$21!$**.

- if a problem asks for $N!$ where $N > 20$, you **cannot** compute it directly in C++. You will need to either:
- output the answer modulo $10^9 + 7$.
- use an array to manually represent massive numbers (BigInt simulation).

NEXT: [[Index]]
