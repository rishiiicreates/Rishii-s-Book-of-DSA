# Buy and Sell with Cooldown

## Problem Statement
- find the maximum profit you can achieve by buying and selling stocks. You may complete as many transactions as you like, but after you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

## Approach / Intuition
- use a [[State Machine]] with [[Dynamic Programming]]. We can define states for holding a stock, not holding (ready to buy), and being in cooldown. Update these states day by day based on possible transitions. Space can be optimized to O(1).

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
int maxProfit(vector<int>& prices) {
    if (prices.size() <= 1) return 0;
    
    int hold = -prices[0];
    int notHold = 0;
    int cooldown = 0;
    
    for (int i = 1; i < prices.size(); ++i) {
        int prevHold = hold;
        int prevNotHold = notHold;
        int prevCooldown = cooldown;
        
        hold = max(prevHold, prevNotHold - prices[i]);
        notHold = max(prevNotHold, prevCooldown);
        cooldown = prevHold + prices[i];
    }
    
    return max(notHold, cooldown);
}
```

## New Keywords / STL Used
- `max`, `vector`

## Edge Cases
- array of size 0 or 1, strictly decreasing prices, strictly increasing prices.

NEXT: [[Index]]
