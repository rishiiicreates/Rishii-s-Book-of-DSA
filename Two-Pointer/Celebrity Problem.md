---
type: concept
tags: [two-pointer, cpp, matrix]
date: 2026-06-30
---
# The Celebrity Problem

## Problem Statement
In a party of `n` people, a celebrity is known by everyone but knows nobody. Given a helper function `knows(a, b)` which tells if `a` knows `b`, find the celebrity.

## Approach / Intuition
Use a [[Two Pointers]] approach initialized at both ends of the population (`left = 0`, `right = n - 1`). If `left` knows `right`, `left` cannot be the celebrity, so `left++`. If `left` does not know `right`, `right` cannot be the celebrity, so `right--`. The remaining person is a potential candidate, which must be verified.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
bool knows(int a, int b); // Given API

int findCelebrity(int n) {
    int left = 0;
    int right = n - 1;
    while (left < right) {
        if (knows(left, right)) {
            left++;
        } else {
            right--;
        }
    }
    int candidate = left;
    for (int i = 0; i < n; ++i) {
        if (i != candidate) {
            if (knows(candidate, i) || !knows(i, candidate)) {
                return -1;
            }
        }
    }
    return candidate;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
No celebrity exists, multiple celebrities (impossible by definition), small party sizes.
