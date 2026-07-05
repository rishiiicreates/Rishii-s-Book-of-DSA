# Longest Valid Parentheses

## Problem Statement
- given a string containing just the characters `(` and `)`, find the length of the longest valid (well-formed) parentheses substring.

## Approach / Intuition
- we use a [[Stack]] to store the indices of the characters. We initialize the stack with `-1` to serve as a base for the first valid substring. As we iterate, if we see `(`, we push its index. If we see `)`, we pop the top of the stack. If the stack becomes empty, we push the current index as a new base. Otherwise, the length of the valid substring ending at the current index is the current index minus the new top of the stack.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the length of the string
- **[[space Complexity]]:** O(N) for storing indices in the stack

## Sample Code
```cpp
#include <string>
#include <stack>
#include <algorithm>
using namespace std;

int longestValidParentheses(string s) {
    stack<int> st;
    st.push(-1);
    int maxLength = 0;
    
    for (int i = 0; i < s.length(); i++) {
        if (s[i] == '(') {
            st.push(i);
        } else {
            st.pop();
            if (st.empty()) {
                st.push(i);
            } else {
                maxLength = max(maxLength, i - st.top());
            }
        }
    }
    return maxLength;
}
```

## New Keywords / STL Used
- `std::stack`, `std::max`

## Edge Cases
- string with no valid pairs, fully valid string, disjoint valid strings like `()()`, overlapping invalid strings like `(()`.

NEXT: [[Index]]
