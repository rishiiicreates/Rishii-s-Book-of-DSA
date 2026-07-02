---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Infix Evaluation

## Problem Statement
Evaluate a given string representing a mathematical infix expression containing numbers and operators `+, -, *, /` along with parentheses.

## Approach / Intuition
We use two [[Stack]]s: one for numbers and another for operators. We iterate through the expression. Numbers are pushed onto the number stack. Opening parentheses go to the operator stack. Operators are pushed onto the operator stack after evaluating and popping operators of higher or equal precedence. For a closing parenthesis, we evaluate until we see an opening parenthesis.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the string
- **[[Space Complexity]]:** O(N) for the two stacks

## Sample Code
```cpp
#include <string>
#include <stack>
#include <cctype>
using namespace std;

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

int applyOp(int a, int b, char op) {
    switch (op) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return a / b;
    }
    return 0;
}

int evaluate(string tokens) {
    stack<int> values;
    stack<char> ops;
    
    for (int i = 0; i < tokens.length(); i++) {
        if (tokens[i] == ' ') continue;
        
        if (tokens[i] == '(') {
            ops.push(tokens[i]);
        } else if (isdigit(tokens[i])) {
            int val = 0;
            while (i < tokens.length() && isdigit(tokens[i])) {
                val = (val * 10) + (tokens[i] - '0');
                i++;
            }
            values.push(val);
            i--;
        } else if (tokens[i] == ')') {
            while (!ops.empty() && ops.top() != '(') {
                int val2 = values.top(); values.pop();
                int val1 = values.top(); values.pop();
                char op = ops.top(); ops.pop();
                values.push(applyOp(val1, val2, op));
            }
            if (!ops.empty()) ops.pop();
        } else {
            while (!ops.empty() && precedence(ops.top()) >= precedence(tokens[i])) {
                int val2 = values.top(); values.pop();
                int val1 = values.top(); values.pop();
                char op = ops.top(); ops.pop();
                values.push(applyOp(val1, val2, op));
            }
            ops.push(tokens[i]);
        }
    }
    
    while (!ops.empty()) {
        int val2 = values.top(); values.pop();
        int val1 = values.top(); values.pop();
        char op = ops.top(); ops.pop();
        values.push(applyOp(val1, val2, op));
    }
    
    return values.top();
}
```

## New Keywords / STL Used
`std::isdigit`, `std::stack`, `switch`

## Edge Cases
Multi-digit numbers, spaces in expression, negative numbers (if applicable), highly nested parentheses.
