# Palindrome

## Problem Statement
- given a string $S$, determine if it reads the same forwards and backwards.

- *example:* $S = \text{"racecar"}$ is a palindrome. $S = \text{"hello"}$ is not.


## Approach: Two Pointers (Optimal)

- instead of creating a reversed copy of the string (which takes $O(N)$ space), we can use the [[Two Pointers]] technique.

- place one pointer `left` at the beginning ($0$) and another pointer `right` at the end ($N - 1$).
- compare the characters at `left` and `right`.
- if they match, move the pointers towards the center (`left++`, `right--`).
- if they mismatch at any point, the string is not a palindrome.
- the loop terminates when `left \ge right`.


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


## Complexity Analysis
- **time Complexity:** $O(N)$. In the worst-case scenario (the string is a palindrome), we iterate through half of the characters, which simplifies to $O(N)$.
- **space Complexity:** $O(1)$. The checking is done strictly in-place without any auxiliary memory allocation.

NEXT: [[Index]]
