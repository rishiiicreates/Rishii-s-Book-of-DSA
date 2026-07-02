---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Check for Balanced Parentheses

## Problem Statement
Given a string containing just the characters `'(', ')', '{', '}', '['` and `']'`, determine if the input string is valid (balanced).

## Approach / Intuition
Iterate through the string character by character. When encountering an opening bracket, push it onto a [[Stack]]. When encountering a closing bracket, check if the stack is not empty and if its top element matches the corresponding opening bracket. If it matches, pop the top element; otherwise, the string is unbalanced. Finally, if the stack is empty, the string is perfectly balanced.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the string
- **[[Space Complexity]]:** O(N) for the stack in the worst case

## Sample Code
```cpp
#include <string>
#include <stack>
using namespace std;

bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') {
            st.push(c);
        } else {
            if (st.empty()) return false;
            char top = st.top();
            st.pop();
            if ((c == ')' && top != '(') || 
                (c == '}' && top != '{') || 
                (c == ']' && top != '[')) {
                return false;
            }
        }
    }
    return st.empty();
}
```

## New Keywords / STL Used
`std::stack`, range-based for loop

## Edge Cases
String with only opening or only closing brackets, empty string, incorrectly nested but equal count of brackets.
