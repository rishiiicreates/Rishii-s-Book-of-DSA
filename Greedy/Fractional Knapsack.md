# Fractional Knapsack

## Problem Statement
- given weights and values of N items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. We can break items for maximizing the total value.

## Approach / Intuition
- calculate the value/weight ratio for each item. Sort the items based on this ratio in descending order. Iteratively pick items with the highest ratio until the knapsack is full. If an item cannot fit completely, take a fraction of it using the [[Greedy Algorithm]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Item {
    int value, weight;
};

bool cmp(Item a, Item b) {
    double r1 = (double)a.value / a.weight;
    double r2 = (double)b.value / b.weight;
    return r1 > r2;
}

double fractionalKnapsack(int W, vector<Item>& items) {
    sort(items.begin(), items.end(), cmp);
    double finalValue = 0.0;
    for (int i = 0; i < items.size(); i++) {
        if (items[i].weight <= W) {
            W -= items[i].weight;
            finalValue += items[i].value;
        } else {
            finalValue += items[i].value * ((double)W / items[i].weight);
            break;
        }
    }
    return finalValue;
}
```

## New Keywords / STL Used
- custom comparator function for `sort`

## Edge Cases
- knapsack capacity 0
- single item larger than capacity

NEXT: [[Index]]
