---
type: concept
tags: [greedy, cpp, gas-station]
date: 2026-06-30
---
# Gas Station

## Problem Statement
There are N gas stations along a circular route, where the amount of gas at station `i` is `gas[i]`. You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from station `i` to its next station `i+1`. Find the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

## Approach / Intuition
First, check if the total gas is less than the total cost; if so, return -1. If a solution exists, it is unique. Track the current gas balance. If the balance drops below 0 at station `i`, it means no station from the previous start to `i` can be the starting point. Reset the start to `i + 1` and balance to 0, using a [[Greedy Algorithm]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>

using namespace std;

int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int total_gas = 0, total_cost = 0;
    int current_gas = 0, start_index = 0;
    for (int i = 0; i < gas.size(); i++) {
        total_gas += gas[i];
        total_cost += cost[i];
        current_gas += gas[i] - cost[i];
        if (current_gas < 0) {
            start_index = i + 1;
            current_gas = 0;
        }
    }
    return total_gas < total_cost ? -1 : start_index;
}
```

## New Keywords / STL Used
- Standard loop variables

## Edge Cases
- Solution requires wrapping around the end of the array
- Single station circuit
