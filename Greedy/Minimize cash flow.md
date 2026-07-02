---
type: concept
tags: [greedy, cpp, minimize-cash-flow]
date: 2026-06-30
---
# Minimize Cash Flow

## Problem Statement
Given a number of friends who have to give or take some amount of money from one another, find out the minimum number of transactions to settle all debts.

## Approach / Intuition
Calculate the net amount for every person. The person with the maximum net debt should pay the person with the maximum net credit to minimize transactions. This is a [[Greedy Algorithm]] approach to cancel out extremes quickly. Recursively or iteratively settle debts.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N^2) in worst case
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int getMinIndex(const vector<int>& amount) {
    int minInd = 0;
    for (int i = 1; i < amount.size(); i++)
        if (amount[i] < amount[minInd]) minInd = i;
    return minInd;
}

int getMaxIndex(const vector<int>& amount) {
    int maxInd = 0;
    for (int i = 1; i < amount.size(); i++)
        if (amount[i] > amount[maxInd]) maxInd = i;
    return maxInd;
}

void settleAmounts(vector<int>& amount) {
    int maxCredit = getMaxIndex(amount);
    int maxDebit = getMinIndex(amount);
    if (amount[maxCredit] == 0 && amount[maxDebit] == 0) return;
    int minVal = min(-amount[maxDebit], amount[maxCredit]);
    amount[maxCredit] -= minVal;
    amount[maxDebit] += minVal;
    settleAmounts(amount);
}
```

## New Keywords / STL Used
- Recursion

## Edge Cases
- All debts are 0
- Single person owes everything
