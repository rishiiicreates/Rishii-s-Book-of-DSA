# Minimum Window Subsequence

## Problem Statement
- given strings `s` and `t`, find the minimum continuous substring in `s` such that `t` is a subsequence of it.

## Approach / Intuition
- use a specialized [[Two Pointers]] approach. Traverse `s` to find a full match of `t` from left to right. Once matched, traverse backwards from the matching end to find the optimal start point. Update the minimum window and resume searching from the character just after the optimal start.

## Time & Space Complexity
- **[[time Complexity]]:** O(S * T) in the worst case, but often closer to O(S).
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
string minWindowSubsequence(string s, string t) {
    int sIdx = 0, tIdx = 0, minLen = INT_MAX, start = -1;
    while (sIdx < s.size()) {
        if (s[sIdx] == t[tIdx]) tIdx++;
        if (tIdx == t.size()) {
            int end = sIdx;
            tIdx--;
            while (tIdx >= 0) {
                if (s[sIdx] == t[tIdx]) tIdx--;
                sIdx--;
            }
            sIdx++;
            if (end - sIdx + 1 < minLen) {
                minLen = end - sIdx + 1;
                start = sIdx;
            }
            tIdx = 0;
        }
        sIdx++;
    }
    return start == -1 ? "" : s.substr(start, minLen);
}
```

## New Keywords / STL Used
- `std::string::substr`

## Edge Cases
- subsequence `t` is not present
- `t` is longer than `s`
- multiple equally short windows (return the first one)

NEXT: [[Index]]
