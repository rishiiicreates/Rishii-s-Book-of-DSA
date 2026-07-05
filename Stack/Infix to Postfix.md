# Infix to Postfix Conversion

## Problem Statement
- given a mathematical expression in **infix notation** (e.g., $A + B * C$), where operators are written between operands, convert it into **postfix notation** (Reverse Polish Notation, e.g., $A B C * +$), where operators follow their operands.


## Approach: Stack-based Parser (Shunting Yard Algorithm)

- the conversion process relies on the mathematical principles of operator **precedence** and **associativity**. By utilizing a [[Stack]] to temporarily hold operators and parentheses, we can reorder the expression linearly.

- **operands:** If the scanned character is an operand (e.g., variable or digit), immediately append it to the postfix output string.
- **left Parenthesis `(`:** Push it onto the stack to establish a new bounding scope of precedence.
- **right Parenthesis `)`:** The scope is closed. Pop all operators from the stack and append them to the output until a matching left parenthesis `(` is encountered. Discard both parentheses.
- **operators (`+`, `-`, `*`, `/`, `^`):**
   - let the current operator be $\theta_{curr}$.
   - while the stack is not empty, and the top element is not `(`, let the top operator be $\theta_{top}$.
   - if $\text{Precedence}(\theta_{top}) > \text{Precedence}(\theta_{curr})$ OR $\left( \text{Precedence}(\theta_{top}) == \text{Precedence}(\theta_{curr}) \text{ and } \theta_{curr} \text{ is Left-Associative} \right)$, then pop $\theta_{top}$ and append it to the output.
   - finally, push $\theta_{curr}$ onto the stack.
- **termination:** After scanning all characters, pop any remaining operators from the stack and append them to the output.

> [!important]
> Exponentiation (`^`) is strictly **Right-Associative**. For example, $A \text{\textasciicircum}} B \text{\textasciicircum}} C$ evaluates as $A \text{\textasciicircum}} (B \text{\textasciicircum}} C)$. Therefore, if $\theta_{curr}$ is `^` and $\theta_{top}$ is `^`, we do **not** pop $\theta_{top}$. 


## Code Implementation

```cpp
#include <string>
#include <stack>
using namespace std;

// Function to map operators to numerical precedence
int getPrecedence(char c) {
    if (c == '^') return 3;
    else if (c == '/' || c == '*') return 2;
    else if (c == '+' || c == '-') return 1;
    else return -1;
}

string infixToPostfix(const string& s) {
    stack<char> st;
    string result;
    
    for (char c : s) {
        // 1. Operand: append directly
        if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9')) {
            result += c;
        } 
        // 2. Left Parenthesis: push scope
        else if (c == '(') {
            st.push('(');
        } 
        // 3. Right Parenthesis: collapse scope
        else if (c == ')') {
            while (!st.empty() && st.top() != '(') {
                result += st.top();
                st.pop();
            }
            if (!st.empty()) st.pop(); // discard '('
        } 
        // 4. Operator: evaluate precedence and associativity
        else {
            while (!st.empty() && st.top() != '(') {
                int precTop = getPrecedence(st.top());
                int precCurr = getPrecedence(c);
                
                if (precTop > precCurr || (precTop == precCurr && c != '^')) {
                    result += st.top();
                    st.pop();
                } else {
                    break;
                }
            }
            st.push(c);
        }
    }
    
    // 5. Termination: pop remaining operators
    while (!st.empty()) {
        result += st.top();
        st.pop();
    }
    
    return result;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$, where $N$ is the length of the string. Every character is scanned exactly once. Every operator or parenthesis is pushed onto the stack exactly once and popped at most once, yielding an amortized strictly linear bound.
- **space Complexity:** $O(N)$ allocated for the `stack` in the worst case (e.g., an expression containing only left parentheses or strictly increasing precedence operators), plus the $O(N)$ string buffer for the `result`.

NEXT: [[Index]]
