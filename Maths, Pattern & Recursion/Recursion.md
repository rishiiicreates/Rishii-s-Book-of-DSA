---
type: concept
tags: [maths, recursion, cpp, introduction]
date: 2026-06-30
---
# Recursion

## What is Recursion?
Recursion is a programming technique where a function **calls itself** directly or indirectly to solve a smaller instance of the same problem. 

Instead of using iterative loops (`for` or `while`), recursion breaks a massive problem down into progressively smaller chunks until it reaches a chunk so small that the answer is obvious. 

---

## The Two Pillars of Recursion
Every valid recursive function must have two components:

1. **The Base Case(s):** The terminating condition. This is the simplest instance of the problem that can be answered directly without further recursive calls. Without a base case, the function will loop infinitely.
2. **The Recursive Step:** The part of the function that breaks the current problem down and makes a call to itself with modified arguments, driving the problem closer to the base case.

### A Classic Example
```cpp
void printCountdown(int n) {
    // 1. Base Case
    if (n == 0) {
        cout << "Liftoff!\n";
        return; 
    }
    
    // 2. Recursive Step
    cout << n << "...\n";
    printCountdown(n - 1); 
}
```

---

## How Recursion Works: The Call Stack

When a function is called, the OS allocates a chunk of memory called a **Stack Frame** on the Call Stack. This frame holds the function's local variables, arguments, and the return address.

When `printCountdown(3)` is called:
1. `printCountdown(3)` executes, prints `3`, and calls `printCountdown(2)`. The function is *paused*.
2. A new frame is pushed for `printCountdown(2)`. It prints `2`, calls `printCountdown(1)`.
3. A new frame is pushed for `printCountdown(1)`. It prints `1`, calls `printCountdown(0)`.
4. `printCountdown(0)` hits the base case, prints `"Liftoff!"` and `return`s.
5. The stack frames then sequentially *pop* off the stack, resuming and instantly finishing the paused parent functions.

> [!warning] 
> **Stack Overflow:** If you forget a base case, or if the input is massively large (e.g., $N = 10^6$), the program will keep pushing stack frames until it runs out of memory. This crashes the program with a **Stack Overflow Error** (or Segfault in C++).

---

## Iteration vs Recursion
Anything you can write with a loop can be written recursively, and vice versa.
- **Iteration (Loops):** Generally faster and uses $O(1)$ auxiliary space.
- **Recursion:** Often cleaner, requires fewer lines of code, and is mandatory for naturally recursive data structures like Trees and Graphs. However, it incurs an $O(N)$ memory cost due to the call stack.
