# Lowest by removing k digits

## Problem Statement
- given a string representing a non-negative integer, remove `k` digits to form the smallest possible integer.

## Approach / Intuition
- we can use a [[Monotonic Stack]] to store the digits of the number. As we iterate through the string, if the current digit is smaller than the top of the stack and we still have `k > 0`, we pop the stack to ensure smaller digits appear at higher place values. Afterwards, handle leading zeros and any remaining `k`.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the length of the string.
- **[[space Complexity]]:** O(N) for storing the characters in the result string which acts as a stack.

## Sample Code
```cpp
string removeKdigits(string num, int k) {
    string ans = "";
    for (char c : num) {
        while (ans.length() && ans.back() > c && k) {
            ans.pop_back();
            k--;
        }
        if (ans.length() || c != '0') {
            ans.push_back(c);
        }
    }
    while (ans.length() && k--) {
        ans.pop_back();
    }
    return ans.empty() ? "0" : ans;
}
```

## New Keywords / STL Used
- `std::string::back()`, `std::string::pop_back()`, `std::string::push_back()`

## Edge Cases
- `k` is greater than or equal to the length of the string (returns "0").
- the string contains leading zeros after removal.
- all digits are in increasing order.

NEXT: [[Index]]
