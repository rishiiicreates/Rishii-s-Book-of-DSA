---
type: concept
tags: [greedy, cpp, min-notes-given-sum]
date: 2026-06-30
---
# Min Notes with Given Sum

## Problem Statement
Given an amount of money, find the minimum number of currency notes required to make that amount, assuming an infinite supply of standard denominations.

## Approach / Intuition
Start from the largest possible denomination. Find how many times it can fit into the remaining amount. Subtract that value and repeat for the next smaller denomination using a [[Greedy Algorithm]]. Note that this works optimally only for standard denomination systems like US/Indian currency.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of denominations
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>

using namespace std;

int minNotes(int amount) {
    vector<int> denominations = {2000, 500, 200, 100, 50, 20, 10, 5, 2, 1};
    int count = 0;
    for (int note : denominations) {
        if (amount == 0) break;
        count += amount / note;
        amount %= note;
    }
    return count;
}
```

## New Keywords / STL Used
- Vector initialization with an initializer list

## Edge Cases
- Amount is 0
- Amount can be formed by a single note
