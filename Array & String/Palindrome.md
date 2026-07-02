---
type: concept
tags: [string, cpp, two-pointers]
date: 2026-06-30
---
# Palindrome

## Problem Statement
Given a string $S$, determine if it reads the same forwards and backwards.

*Example:* $S = \text{"racecar"}$ is a palindrome. $S = \text{"hello"}$ is not.

---

## Approach: Two Pointers (Optimal)

Instead of creating a reversed copy of the string (which takes $O(N)$ space), we can use the [[Two Pointers]] technique.

1. Place one pointer `left` at the beginning ($0$) and another pointer `right` at the end ($N - 1$).
2. Compare the characters at `left` and `right`.
3. If they match, move the pointers towards the center (`left++`, `right--`).
4. If they mismatch at any point, the string is not a palindrome.
5. The loop terminates when `left \ge right`.

---

## Code Implementation

```cpp
#include <string>
using namespace std;

bool isPalindrome(const string& s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s[left] != s[right]) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}
```

> [!warning]
> Edge cases like an empty string (`""`) or a single-character string (`"a"`) are inherently palindromes. The above logic handles them seamlessly because the `while (left < right)` condition immediately evaluates to `false`, returning `true`.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. In the worst-case scenario (the string is a palindrome), we iterate through half of the characters, which simplifies to $O(N)$.
- **Space Complexity:** $O(1)$. The checking is done strictly in-place without any auxiliary memory allocation.
