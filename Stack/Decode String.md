---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Decode String

## Problem Statement
Decode an encoded string where the pattern `k[encoded_string]` indicates that `encoded_string` is repeated exactly `k` times.

## Approach / Intuition
We can process this using two [[Stack]]s: one for repeat counts and one for the string fragments built so far. We parse the string, pushing numbers and current strings on encountering `[`. Upon encountering `]`, we pop the last string and its repeat count, append the current string `k` times, and update the state.

## Time & Space Complexity
- **[[Time Complexity]]:** O(Max(K) * N) where K is the multiplier value and N is the string length.
- **[[Space Complexity]]:** O(N) for the recursion depth or the stacks.

## Sample Code
```cpp
string decodeString(string s) {
    stack<int> countStack;
    stack<string> stringStack;
    string currentString = "";
    int k = 0;
    for (char ch : s) {
        if (isdigit(ch)) {
            k = k * 10 + (ch - '0');
        } else if (ch == '[') {
            countStack.push(k);
            stringStack.push(currentString);
            currentString = "";
            k = 0;
        } else if (ch == ']') {
            string decodedString = stringStack.top();
            stringStack.pop();
            int currentK = countStack.top();
            countStack.pop();
            while (currentK-- > 0) decodedString += currentString;
            currentString = decodedString;
        } else {
            currentString += ch;
        }
    }
    return currentString;
}
```

## New Keywords / STL Used
- `std::stack`, `std::isdigit`

## Edge Cases
- Nested brackets like `2[3[a]b]`.
- Modifiers `k` with multiple digits (e.g., `10[a]`).
- Empty string or string without any brackets.
