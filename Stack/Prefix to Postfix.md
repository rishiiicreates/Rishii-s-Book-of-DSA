# Prefix to Postfix Conversion

## Problem Statement
- given a mathematical expression in **prefix notation** (Polish Notation, e.g., $* + A B C$), where operators precede their operands, convert it strictly into **postfix notation** (Reverse Polish Notation, e.g., $A B + C *$).


## Approach: Right-to-Left Stack Evaluation

- the topology of prefix notation allows it to be evaluated intuitively if we traverse the string in reverse (from right to left). By reading operands first, we ensure that by the time an operator is encountered, both its required operands are already available on the [[Stack]].

- **traverse Backward:** Iterate through the prefix string from index $N-1$ down to $0$.
- **operands:** If the current character is an operand, push it onto the stack as a string. (It represents a trivial sub-expression).
- **operators:** If the current character is an operator $\theta$:
   - pop the top two elements from the stack. Because we read from right to left, the element sitting at the top of the stack is the **first operand** ($Op_1$), and the element directly beneath it is the **second operand** ($Op_2$).
   - form the composite postfix string: `Op1 + Op2 + operator`.
   - push this new composite string back onto the stack.
- **termination:** When the loop finishes, the stack will contain exactly one string: the fully converted postfix expression.

> [!important]
> When popping operands for a binary operator $\theta$ in right-to-left traversal, $Op_1 = \text{stack.pop()}$ and $Op_2 = \text{stack.pop()}$. The semantic ordering for postfix requires the structure $(Op_1 \parallel Op_2 \parallel \theta)$. Do not invert the operands.


## Code Implementation

```cpp
#include <string>
#include <stack>
#include <algorithm>
using namespace std;

bool isOperator(char x) {
    return (x == '+' || x == '-' || x == '*' || x == '/' || x == '^');
}

string prefixToPostfix(const string& pre_exp) {
    stack<string> st;
    int n = pre_exp.length();

    // Traverse from right to left
    for (int i = n - 1; i >= 0; i--) {
        char c = pre_exp[i];

        if (isOperator(c)) {
            // Operator encountered: pop two operands
            string op1 = st.top(); st.pop();
            string op2 = st.top(); st.pop();

            // Form new postfix expression: Op1 Op2 Operator
            string temp = op1 + op2 + c;
            
            // Push back to stack
            st.push(temp);
        } else {
            // Operand encountered: convert char to string and push
            st.push(string(1, c));
        }
    }

    // The single remaining string is the complete postfix expression
    return st.top();
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ in terms of operations, but string concatenations `temp = op1 + op2 + c` may cause reallocation. Since the final string length is $N$, the overall execution mathematically sums to $O(N)$ when string lengths geometrically scale, bounded asymptotically by the length of the string.
- **space Complexity:** $O(N)$ dynamically allocated to the `stack` to hold string segments, which culminate in a single string of length $N$.

NEXT: [[Index]]
