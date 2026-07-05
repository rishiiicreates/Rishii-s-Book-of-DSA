# Evaluate Postfix (Reverse Polish Notation)

## Problem Statement
- given a mathematical expression structurally encoded as an array of discrete topological tokens in Postfix Notation (Reverse Polish Notation), evaluate the arithmetic scalar result. Operators strictly guarantee binary arity (addition, subtraction, multiplication, integer division).


## Approach: LIFO Operand Reduction

- in conventional Infix notation, order of operations mandates operator precedence (BODMAS) and nested geometric parentheses. Postfix notation fundamentally bypasses precedence rules; operators act structurally on the two most recently observed operands.
- this maps perfectly to the Last-In-First-Out (LIFO) temporal axioms of the Stack ADT.

- **state Machine Execution:**
- we iterate sequentially through the mathematical tokens:
- **operand (Scalar):** Parse the string into an integer and strictly `push` it onto the stack state.
- **operator (BinOp):**
   - `pop` the absolute topmost scalar as Operand 2 ($Y$).
   - `pop` the subsequent topmost scalar as Operand 1 ($X$).
   - evaluate the topological equation $Z = X \text{ BinOp } Y$.
   - `push` the resolved scalar $Z$ back onto the stack.

- upon exhausting the token sequence, the mathematical system state collapses exactly to a single remaining scalar within the stack. This is the terminal arithmetic result.


## Code Implementation

```cpp
#include <vector>
#include <string>
#include <stack>
#include <stdexcept>

using namespace std;

int evalRPN(vector<string>& tokens) {
    stack<int> st;
    
    for (const string& token : tokens) {
        // Operator Dispatch Identification
        if (token == "+" || token == "-" || token == "*" || token == "/") {
            // LIFO Operand resolution
            int y = st.top(); st.pop();
            int x = st.top(); st.pop();
            
            // Arithmetic evaluation
            if (token == "+") st.push(x + y);
            else if (token == "-") st.push(x - y);
            else if (token == "*") st.push(x * y);
            else if (token == "/") st.push(x / y); // C++ standard truncation toward zero
        } else {
            // Topological Operand Injection
            st.push(stoi(token));
        }
    }
    
    // Terminal System Collapse
    return st.top();
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ strictly linear time. We map each token to a single state transition (bounded by $O(1)$ operations).
- **space Complexity:** $O(N)$ bound. In the worst case (all operands first), the stack consumes memory linearly proportional to the token set cardinality.

> [!warning]
> **Non-Commutative Division & Subtraction:** The order of scalar extraction is mathematically paramount! Because a stack extracts the *last* inserted element first, the topmost element mathematically represents the right-hand side of the binomial operator ($Y$), not the left-hand side ($X$). Flipping $X$ and $Y$ catastrophically invalidates subtraction and integer division topologies.

NEXT: [[Index]]
