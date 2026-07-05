# Check Redundant Brackets

## Problem Statement
- check if a given mathematical expression contains redundant brackets. A pair of brackets is redundant if it doesn't enclose any useful operators between them.

## Approach / Intuition
- we can parse the expression using a [[Stack]]. Push all characters onto the stack until we hit a closing bracket `)`. Then, pop elements until the matching `(` is found. If no mathematical operator (`+`, `-`, `*`, `/`) is encountered during this popping phase, the brackets are redundant.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the length of the string, as each character is pushed and popped at most once.
- **[[space Complexity]]:** O(N) for maintaining the stack.

## Sample Code
```cpp
bool checkRedundancy(string s) {
    stack<char> st;
    for (char ch : s) {
        if (ch == ')') {
            char top = st.top();
            st.pop();
            bool hasOperator = false;
            while (top != '(') {
                if (top == '+' || top == '-' || top == '*' || top == '/') {
                    hasOperator = true;
                }
                top = st.top();
                st.pop();
            }
            if (!hasOperator) return true;
        } else {
            st.push(ch);
        }
    }
    return false;
}
```

## New Keywords / STL Used
- `std::stack`

## Edge Cases
- expressions with multiple nested non-redundant brackets like `((a+b)*(c+d))`.
- single variables enclosed in brackets like `(a)`.
- expressions devoid of brackets entirely.

NEXT: [[Index]]
