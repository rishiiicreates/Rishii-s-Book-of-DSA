---
type: concept
tags: [fundamentals, cpp, loop]
date: 2026-06-30
---
# While Loop

## What is a While Loop?
A `while` loop executes a block of code repeatedly as long as a specified boolean condition evaluates to `true`. Unlike a `for` loop, which is typically used when the exact number of iterations is known, a `while` loop is best used when the termination depends on a dynamic state or input condition.

---

## The Standard `while` Loop

The condition is evaluated **before** the block of code executes. If the condition is initially `false`, the code inside the loop will never run.

```cpp
int count = 5;

// The loop will run exactly 5 times
while (count > 0) {
    cout << count << " ";
    count--; // Crucial: must update the state to eventually reach false
}
// Output: 5 4 3 2 1
```

> [!warning] 
> **Infinite Loops:** If the state variables involved in the boolean condition are never updated inside the loop, the condition will never become false, causing the program to freeze or crash (e.g., Time Limit Exceeded).

---

## Common Use Cases in DSA

### 1. Two-Pointer Algorithms
`while` loops are the standard structure for two-pointer traversals, where left and right bounds converge.
```cpp
int left = 0, right = arr.size() - 1;
while (left < right) {
    if (arr[left] + arr[right] == target) return true;
    else if (arr[left] + arr[right] < target) left++;
    else right--;
}
```

### 2. Reading Input until EOF
When processing competitive programming problems with unknown amounts of input, the `while` loop handles extraction perfectly.
```cpp
int x;
// Returns true as long as extraction is successful, false on End of File
while (cin >> x) {
    process(x);
}
```

---

## The `do-while` Loop
A `do-while` loop is a variant where the condition is checked **after** the loop body executes. This guarantees that the code inside the loop will execute **at least once**, regardless of whether the condition is true or false.

```cpp
int option = 0;
do {
    cout << "Enter 1 to continue, 0 to exit: ";
    cin >> option;
} while (option != 0); 
```

---

## Loop Control Statements
Just like `for` loops, you can use:
- **`break`**: Exits the loop entirely.
- **`continue`**: Skips the rest of the current iteration. 
  - *Warning:* If you use `continue` in a `while` loop, you must manually update your loop variables *before* the `continue` statement, otherwise you will trigger an infinite loop!

---

## Complexity
- **Time Complexity:** $O(N)$ where $N$ is the number of times the condition evaluates to true.
- **Space Complexity:** $O(1)$ auxiliary space.
