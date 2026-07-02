---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Stock Span Problem

## Problem Statement
Calculate the stock span for each day, which is the maximum number of consecutive days just before the given day (including itself) for which the stock price was less than or equal to the current day's price.

## Approach / Intuition
This is a variation of finding the [[Previous Greater Element]]. We can use a [[Stack]] to store the indices of the days. While iterating, if the current price is greater than or equal to the price at the index on the top of the stack, we pop the stack. The span is the difference between the current index and the index now at the top of the stack.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) since elements are processed in the stack a constant number of times.
- **[[Space Complexity]]:** O(N) for the stack storing the indices.

## Sample Code
```cpp
vector<int> calculateSpan(int price[], int n) {
    vector<int> span(n);
    stack<int> st;
    for(int i = 0; i < n; i++){
        while(!st.empty() && price[st.top()] <= price[i]){
            st.pop();
        }
        span[i] = st.empty() ? (i + 1) : (i - st.top());
        st.push(i);
    }
    return span;
}
```

## New Keywords / STL Used
- `std::stack`, `std::vector`

## Edge Cases
- Strictly decreasing stock prices (all spans are 1).
- Strictly increasing stock prices.
- Duplicate adjacent prices.
