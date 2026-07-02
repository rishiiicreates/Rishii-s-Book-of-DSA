---
type: concept
tags: [fundamentals, cpp, conditionals]
date: 2026-06-30
---
# Conditional Statements

## What are Conditional Statements?
Conditional statements allow a program to execute different blocks of code based on specific boolean logic. They evaluate an expression to `true` or `false` and direct the flow of execution accordingly. 

This branching mechanism is the foundation of decision-making in algorithms.

---

## The `if-else` Construct

The standard way to handle conditions in C++ is using the `if`, `else if`, and `else` blocks. The program checks the conditions sequentially from top to bottom. The first one that evaluates to `true` has its block executed, and the rest are skipped.

```cpp
int score = 85;

if (score >= 90) {
    cout << "Grade: A\n";
} else if (score >= 80) {
    cout << "Grade: B\n";
} else if (score >= 70) {
    cout << "Grade: C\n";
} else {
    cout << "Grade: F\n"; // Fallback if nothing else matches
}
```

> [!warning] 
> **Equality vs Assignment Operator:** A very common beginner bug in C++ is writing `if (x = 5)` instead of `if (x == 5)`. The single `=` is an assignment operator, which sets `x` to 5 and evaluates to `true`, completely breaking the intended logic. Always use `==` for comparison!

---

## The Ternary Operator (`? :`)
For simple `if-else` assignments, C++ offers a concise, one-line alternative called the ternary operator. 

**Syntax:** `condition ? value_if_true : value_if_false;`

```cpp
int a = 10, b = 20;

// Standard if-else
int max_val;
if (a > b) max_val = a;
else max_val = b;

// Ternary equivalent (Clean & Fast)
int max_val_ternary = (a > b) ? a : b;
```

---

## The `switch` Statement
When you need to compare a single integer or character against multiple exact, distinct values, a `switch` statement is much cleaner than a long chain of `else if` statements.

```cpp
char direction = 'W';

switch (direction) {
    case 'N':
        cout << "Moving North\n";
        break; // Crucial: prevents falling through to the next case
    case 'S':
        cout << "Moving South\n";
        break;
    case 'E':
        cout << "Moving East\n";
        break;
    case 'W':
        cout << "Moving West\n";
        break;
    default:
        cout << "Invalid direction\n";
}
```

> [!tip] 
> **Early Returns in Functions:** Instead of heavily nesting `if-else` statements, professional C++ code often uses "early returns" or "guard clauses" to handle edge cases immediately, keeping the main logic flat and readable.
> ```cpp
> void processUser(int age) {
>     if (age < 18) return; // Guard clause
>     // The rest of the function assumes age >= 18
> }
> ```

---

## Complexity
- **Time Complexity:** $O(1)$. Evaluating a standard integer or boolean condition takes constant time.
- **Space Complexity:** $O(1)$. No extra space is required for branching.
