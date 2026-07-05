# Generate Numbers with given digits

## Problem Statement
- generate the first `n` numbers using only specific digits (e.g., 5 and 6) in increasing order.

## Approach / Intuition
- use a [[Queue]] starting with the base digits as strings. In each step, pop the front element, add it to the result, and then push new elements by appending the base digits to the popped string. This acts like a level-order traversal of a tree generating the combinations.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(n)

## Sample Code
```cpp
vector<string> generateNumbers(int n) {
    vector<string> res;
    if (n == 0) return res;
    queue<string> q;
    q.push("5");
    q.push("6");
    for (int i = 0; i < n; ++i) {
        string curr = q.front();
        q.pop();
        res.push_back(curr);
        q.push(curr + "5");
        q.push(curr + "6");
    }
    return res;
}
```

## New Keywords / STL Used
- `std::to_string` (implied with concatenation), `std::queue`

## Edge Cases
- `n = 0`, large values of `n` causing string length inflation.

NEXT: [[Index]]
