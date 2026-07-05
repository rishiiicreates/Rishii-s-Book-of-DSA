# Buy and Sell – Max 2 Transactions Allowed

## Problem Statement
- find the maximum profit from buying and selling stocks given an array of prices. You may complete at most two transactions, and you must sell the stock before buying again.

## Approach / Intuition
- use a [[State Machine]] to track the four possible states: buying the first stock, selling the first stock, buying the second stock, and selling the second stock. We can update these states sequentially for each day to maximize profit using [[Dynamic Programming]] concepts space-optimized to O(1).

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
int maxProfit(vector<int>& prices) {
    int buy1 = -1e9, sell1 = 0;
    int buy2 = -1e9, sell2 = 0;
    
    for (int p : prices) {
        buy1 = max(buy1, -p);
        sell1 = max(sell1, buy1 + p);
        buy2 = max(buy2, sell1 - p);
        sell2 = max(sell2, buy2 + p);
    }
    
    return sell2;
}
```

## New Keywords / STL Used
- `max`, `vector`

## Edge Cases
- decreasing prices (profit 0), array length less than 2.

NEXT: [[Index]]
