---
type: concept
tags: [fundamentals, cpp, loops]
date: 2026-06-30
---
# For Loop

## What is a For Loop?
A `for` loop is a control flow statement used to execute a block of code repeatedly for a known number of times. It is the most commonly used loop in Data Structures and Algorithms when iterating through arrays, matrices, or simulating sequential operations.

---

## The Standard `for` Loop

A standard `for` loop condenses three critical pieces of iteration logic into a single line:
1. **Initialization:** Executed once at the beginning (e.g., `int i = 0`).
2. **Condition:** Checked before *every* iteration. If `true`, the loop runs. (e.g., `i < n`).
3. **Update:** Executed at the very end of every iteration (e.g., `i++`).

```cpp
int n = 5;
for (int i = 0; i < n; ++i) {
    cout << i << " ";
}
// Output: 0 1 2 3 4
```

> [!tip]
> Pre-increment (`++i`) is marginally faster than post-increment (`i++`) in C++ when dealing with complex iterators or objects because it avoids creating a temporary copy of the object. While the compiler optimizes this for standard integers, it's a good habit to write `++i` in competitive programming.

---

## The Range-Based `for` Loop (C++11)

When you simply need to visit every element in a container (like a `vector`, `set`, or `map`) from start to finish, the range-based for loop is drastically cleaner and less error-prone.

```cpp
#include <vector>
using namespace std;

vector<int> arr = {10, 20, 30, 40};

// Standard way
for (int i = 0; i < arr.size(); ++i) {
    cout << arr[i] << " ";
}

// Range-based way (Clean & modern)
for (int val : arr) {
    cout << val << " ";
}
```

### Passing by Reference in Range Loops
If you want to **modify** the elements in the container, or if the container holds massive objects (like strings or other vectors) that are expensive to copy, you must pass by reference using `&`.

```cpp
vector<string> names = {"Alice", "Bob"};

// Pass by reference to avoid copying strings, and use 'const' since we don't modify them
for (const string& name : names) {
    cout << name << "\n";
}

// Modifying elements in-place
vector<int> nums = {1, 2, 3};
for (int& num : nums) {
    num *= 2; // the actual vector becomes {2, 4, 6}
}
```

---

## Loop Control Statements
- **`break`**: Immediately terminates the entire loop. Execution resumes at the first statement following the loop.
- **`continue`**: Immediately skips the rest of the current iteration and jumps directly to the **Update** step (`i++`).

```cpp
for (int i = 0; i < 10; ++i) {
    if (i % 2 == 0) continue; // Skip even numbers
    if (i == 7) break;        // Stop the loop entirely when i is 7
    cout << i << " ";
}
// Output: 1 3 5 
```

---

## Complexity
- **Time Complexity:** $O(N)$ where $N$ is the number of times the condition evaluates to true.
- **Space Complexity:** $O(1)$ auxiliary space, as we typically only allocate the single loop counter variable `i`.
