# Redundant Brackets

## Problem Statement
- given a valid mathematical expression, check if it contains any redundant (unnecessary) brackets.

## Approach / Intuition
- we use a [[Stack]] to keep track of characters. We push every character onto the stack except the closing bracket `)`. When we encounter `)`, we pop elements from the stack until we find the matching `(`. If we pop only `(` immediately without any operator `+`, `-`, `*`, or `/` in between, it implies the brackets were redundant.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the length of the string
- **[[space Complexity]]:** O(N) to store characters in the stack

## Sample Code
```cpp
#include <string>
#include <stack>
using namespace std;

bool checkRedundantBrackets(string &s) {
    stack<char> st;
    for (int i = 0; i < s.length(); i++) {
        if (s[i] == ')') {
            bool hasOperator = false;
            while (!st.empty() && st.top() != '(') {
                char top = st.top();
                if (top == '+' || top == '-' || top == '*' || top == '/') {
                    hasOperator = true;
                }
                st.pop();
            }
            st.pop();
            if (!hasOperator) {
                return true;
            }
        } else {
            st.push(s[i]);
        }
    }
    return false;
}
```

## New Keywords / STL Used
- `std::stack`

## Edge Cases
- expression entirely enclosed in redundant brackets like `((a+b))`, single variables enclosed in brackets `(a)`.

NEXT: [[Index]]
