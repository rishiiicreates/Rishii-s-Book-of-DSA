---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Replace Adjacent Opposite

## Problem Statement
Remove adjacent characters that are of the same letter but opposite cases (e.g., 'a' and 'A'). This process is repeated until no such adjacent pairs exist.

## Approach / Intuition
We can use a string as a [[Stack]]. We iterate over the characters, and if the current character forms an adjacent opposite pair with the last character in the result string, we pop it. Otherwise, we push the current character. 

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) as we iterate through the characters of the string once.
- **[[Space Complexity]]:** O(N) to store the characters in the result string representing the stack.

## Sample Code
```cpp
string makeGood(string s) {
    string ans = "";
    for (char c : s) {
        if (!ans.empty() && abs(ans.back() - c) == 32) {
            ans.pop_back();
        } else {
            ans.push_back(c);
        }
    }
    return ans;
}
```

## New Keywords / STL Used
- `std::abs`, `std::string::back()`, `std::string::pop_back()`

## Edge Cases
- A string that gets completely erased.
- A string with no adjacent opposites.
- Strings that resolve recursively after multiple removals.
