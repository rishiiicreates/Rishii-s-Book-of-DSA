---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Score of Balanced Parentheses

## Problem Statement
Given a balanced parentheses string, compute the score of the string based on the following rules: `()` has score 1, `AB` has score A + B, and `(A)` has score 2 * A.

## Approach / Intuition
We maintain a [[Stack]] to keep track of the scores at the current depth. When we see `(`, we push a 0 to indicate a new nested level. When we see `)`, we pop the inner score. If the inner score is 0, it means it was an empty `()`, so we add 1 to the new top of the stack. If it's greater than 0, it was `(A)`, so we add `2 * innerScore` to the new top of the stack.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the string length
- **[[Space Complexity]]:** O(N) for the stack depth

## Sample Code
```cpp
#include <string>
#include <stack>
using namespace std;

int scoreOfParentheses(string s) {
    stack<int> st;
    st.push(0);
    
    for (char c : s) {
        if (c == '(') {
            st.push(0);
        } else {
            int v = st.top();
            st.pop();
            int w = st.top();
            st.pop();
            st.push(w + max(2 * v, 1));
        }
    }
    return st.top();
}
```

## New Keywords / STL Used
`std::stack`, `std::max`

## Edge Cases
Deeply nested string `((()))`, consecutive strings `()()()`, combinations like `(()(()))`.
