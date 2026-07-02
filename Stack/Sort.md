---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Sort a Stack

## Problem Statement
Given a stack, sort it such that the top of the stack contains the greatest element.

## Approach / Intuition
We can sort the [[Stack]] using [[Recursion]]. We recursively empty the stack, holding the popped elements in the function call stack. Once the stack is empty, we start inserting elements back one by one in sorted order using another recursive function that pushes elements below any larger elements currently in the stack.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N^2) where N is the number of elements
- **[[Space Complexity]]:** O(N) for the recursion stack

## Sample Code
```cpp
#include <stack>
using namespace std;

void insertSorted(stack<int>& st, int element) {
    if (st.empty() || st.top() <= element) {
        st.push(element);
        return;
    }
    
    int temp = st.top();
    st.pop();
    insertSorted(st, element);
    st.push(temp);
}

void sortStack(stack<int>& st) {
    if (st.empty()) {
        return;
    }
    
    int temp = st.top();
    st.pop();
    sortStack(st);
    insertSorted(st, temp);
}
```

## New Keywords / STL Used
`std::stack`

## Edge Cases
Empty stack, stack with a single element, already sorted stack, reverse sorted stack.
