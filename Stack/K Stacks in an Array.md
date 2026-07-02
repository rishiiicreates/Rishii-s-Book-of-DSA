---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# K Stacks in an Array

## Problem Statement
Implement `k` stacks using a single array such that the space is dynamically shared among all the stacks.

## Approach / Intuition
We use three arrays: `arr` for storing actual elements, `top` for storing the index of the top element of each [[Stack]], and `next` for linking the elements of the same stack and tracking the next free spot. A variable `freeSpot` points to the first available index. When pushing, we update `next` to point to the old top, and update the new top to the allocated index.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for push and pop operations
- **[[Space Complexity]]:** O(N + K) where N is array capacity and K is number of stacks

## Sample Code
```cpp
#include <vector>
using namespace std;

class KStacks {
    int *arr;
    int *top;
    int *next;
    int n, k;
    int freeSpot;
public:
    KStacks(int k, int n) {
        this->k = k;
        this->n = n;
        arr = new int[n];
        top = new int[k];
        next = new int[n];
        
        for (int i = 0; i < k; i++) top[i] = -1;
        for (int i = 0; i < n - 1; i++) next[i] = i + 1;
        next[n - 1] = -1;
        
        freeSpot = 0;
    }
    
    ~KStacks() {
        delete[] arr;
        delete[] top;
        delete[] next;
    }

    bool push(int x, int m) {
        if (freeSpot == -1) return false;
        
        int index = freeSpot;
        freeSpot = next[index];
        arr[index] = x;
        next[index] = top[m - 1];
        top[m - 1] = index;
        return true;
    }

    int pop(int m) {
        if (top[m - 1] == -1) return -1;
        
        int index = top[m - 1];
        top[m - 1] = next[index];
        next[index] = freeSpot;
        freeSpot = index;
        return arr[index];
    }
};
```

## New Keywords / STL Used
`new`, `delete[]`, pointer arithmetic

## Edge Cases
Array becomes full (no free spot), a specific stack is empty and pop is called on it.
