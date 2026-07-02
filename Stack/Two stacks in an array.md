---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Two Stacks in an Array

## Problem Statement
Implement two stacks in a single array such that both stacks can perform `push` and `pop` operations without overflowing as long as there is any free space in the array.

## Approach / Intuition
Instead of dividing the array into two fixed halves, we start the first [[Stack]] from the beginning of the array and the second stack from the end. They grow towards each other. This ensures optimal space utilization and prevents overflow of one stack if the other is empty while total space is still available.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for all operations
- **[[Space Complexity]]:** O(N) total space for the array

## Sample Code
```cpp
#include <vector>
using namespace std;

class TwoStacks {
    int *arr;
    int size;
    int top1, top2;
public:
    TwoStacks(int n) {
        size = n;
        arr = new int[n];
        top1 = -1;
        top2 = size;
    }
    
    ~TwoStacks() {
        delete[] arr;
    }

    void push1(int x) {
        if (top1 < top2 - 1) {
            top1++;
            arr[top1] = x;
        }
    }

    void push2(int x) {
        if (top1 < top2 - 1) {
            top2--;
            arr[top2] = x;
        }
    }

    int pop1() {
        if (top1 >= 0) {
            int x = arr[top1];
            top1--;
            return x;
        }
        return -1;
    }

    int pop2() {
        if (top2 < size) {
            int x = arr[top2];
            top2++;
            return x;
        }
        return -1;
    }
};
```

## New Keywords / STL Used
`new`, `delete[]`, `class`

## Edge Cases
Pushing when `top1 == top2 - 1` (Array completely full), popping when respective stack is empty.
