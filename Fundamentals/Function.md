---
type: concept
tags: [fundamentals, cpp, function]
date: 2026-06-30
---
# Function

## What is a Function?
A Function is an encapsulated, reusable block of code that takes some inputs (arguments), performs specific operations, and optionally returns an output. Functions break down massive, monolithic algorithms into manageable, modular, and readable pieces.

---

## Function Anatomy

```cpp
// Return Type | Name | Parameters (Arguments)
int addNumbers(int a, int b) {
    // Function Body
    int sum = a + b;
    return sum; // Return Statement
}
```
- **Return Type:** Defines the data type the function will pass back (`int`, `double`, `string`, etc.). If it returns nothing, use `void`.
- **Parameters:** Local variables populated by the arguments passed in by the caller.

---

## Pass by Value vs Pass by Reference

This is one of the most critical concepts for performance in C++ competitive programming.

### 1. Pass by Value (The Default)
By default, C++ passes arguments by value. The compiler creates a completely new **copy** of the variable and gives it to the function.
- **Pros:** The original data cannot be accidentally modified.
- **Cons:** Copying massive data structures (like a `vector<int>` with $10^6$ elements) takes $O(N)$ time and $O(N)$ space. Doing this in a loop will instantly cause a Time Limit Exceeded (TLE) error.

### 2. Pass by Reference (`&`)
By appending an ampersand `&` to the type, we pass the memory address of the original variable. No copy is made. The function operates directly on the caller's memory.
- **Pros:** Takes $O(1)$ time and $O(1)$ space, regardless of the object's size.
- **Cons:** The function can modify the caller's data (which is sometimes exactly what you want!).

```cpp
#include <vector>

// BAD: Takes O(N) time and memory to copy the entire array
int sumArray(std::vector<int> arr) { /*...*/ }

// GOOD: Takes O(1) time and memory. The 'const' keyword ensures 
// we don't accidentally modify the array inside the function.
int sumArrayFast(const std::vector<int>& arr) { /*...*/ }
```

> [!important]
> **Best Practice:** For fundamental types (`int`, `char`, `double`, `bool`), Pass by Value is fine because they fit in CPU registers. For complex objects (`string`, `vector`, custom classes), **always** Pass by Reference. If the function shouldn't modify the object, use a `const` reference (`const vector<int>&`).

---

## Advanced Functions

### Lambda Expressions (C++11)
Lambdas allow you to define anonymous, inline functions on the fly. They are incredibly useful for providing custom sorting logic to the `std::sort` function.

```cpp
vector<int> nums = {5, 2, 8, 1};

// Sort in descending order using a lambda expression
sort(nums.begin(), nums.end(), [](int a, int b) {
    return a > b; 
});
```
*The `[]` is the capture clause, allowing the lambda to access local variables from the surrounding scope.*

### Inline Functions
Prepending the `inline` keyword to a short function asks the compiler to replace every function call with the actual code of the function, avoiding the CPU overhead of jumping to a different memory address. (Modern compilers usually do this automatically for small functions).

---

## Complexity
- **Time Complexity:** The overhead of actually calling a function is $O(1)$. The overall complexity depends purely on the logic inside the body.
- **Space Complexity:** Every function call consumes $O(1)$ memory on the **Call Stack** to hold parameters and local variables. If a function calls itself recursively $N$ times, the space complexity becomes $O(N)$.
