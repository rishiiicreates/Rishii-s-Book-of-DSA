# Longest Prefix Suffix

## Problem Statement
- find the length of the longest proper prefix of a string which is also a suffix.

## Approach / Intuition
- we build the LPS array used in the [[KMP Algorithm]]. We maintain the length of the previous longest prefix suffix and compare the current character with the character at that length.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> computeLPSArray(string s) {
    int n = s.length();
    vector<int> lps(n, 0);
    int len = 0;
    int i = 1;
    while (i < n) {
        if (s[i] == s[len]) {
            len++;
            lps[i++] = len;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i++] = 0;
            }
        }
    }
    return lps;
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- string of length 1, all characters same.

NEXT: [[Index]]
