# Longest Subarray Length

## Problem Statement
- find the length of the longest valid (well-formed) parentheses substring.

## Approach / Intuition
- a [[Stack]] is used to keep track of indices of parentheses. We initialize the stack with `-1` to serve as a base for the first valid substring. Upon encountering `(`, we push its index. Upon encountering `)`, we pop. If the stack becomes empty, we push the current index to act as a new base. Otherwise, the current length is the difference between the current index and the new top of the stack.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) as each character's index is pushed and popped at most once.
- **[[space Complexity]]:** O(N) for maintaining indices in the stack.

## Sample Code
```cpp
int longestValidParentheses(string s) {
    stack<int> st;
    st.push(-1);
    int maxLen = 0;
    for (int i = 0; i < s.length(); i++) {
        if (s[i] == '(') {
            st.push(i);
        } else {
            st.pop();
            if (st.empty()) {
                st.push(i);
            } else {
                maxLen = max(maxLen, i - st.top());
            }
        }
    }
    return maxLen;
}
```

## New Keywords / STL Used
- `std::stack`, `std::max`

## Edge Cases
- only left or only right parentheses.
- alternating unmatching parentheses like `)()( `.
- completely empty string.

NEXT: [[Index]]
