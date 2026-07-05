# Evaluation of Postfix Expression

## Problem Statement
- given an array of strings representing an arithmetic expression in Reverse Polish Notation (Postfix), evaluate the expression and return its value.

## Approach / Intuition
- we use a [[Stack]] to store operands. We iterate through the given postfix tokens. If the token is a number, we push it onto the stack. If it is an operator, we pop the top two numbers from the stack, apply the operator to them, and push the result back onto the stack. At the end, the stack will contain exactly one element, which is the final answer.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the number of tokens
- **[[space Complexity]]:** O(N) to store the operands in the stack

## Sample Code
```cpp
#include <vector>
#include <string>
#include <stack>
using namespace std;

int evalRPN(vector<string>& tokens) {
    stack<int> st;
    for (const string& token : tokens) {
        if (token == "+" || token == "-" || token == "*" || token == "/") {
            int b = st.top(); st.pop();
            int a = st.top(); st.pop();
            if (token == "+") st.push(a + b);
            else if (token == "-") st.push(a - b);
            else if (token == "*") st.push(a * b);
            else if (token == "/") st.push(a / b);
        } else {
            st.push(stoi(token));
        }
    }
    return st.top();
}
```

## New Keywords / STL Used
- `std::stoi`, `std::stack`

## Edge Cases
- single operand expression, division by negative numbers, zero division (if possible in constraints).

NEXT: [[Index]]
