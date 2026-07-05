# Z algorithm

## Problem Statement
- find all occurrences of a pattern in a text using linear time complexity.

## Approach / Intuition
- the [[Z Algorithm]] computes a Z-array for a combined string `pattern + "$" + text`. The value `Z[i]` represents the length of the longest substring starting from `i` that is also a prefix of the string.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N + M)$
- **[[space Complexity]]:** $O(N + M)$

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> zAlgorithm(string text, string pattern) {
    string concat = pattern + "$" + text;
    int l = concat.length();
    vector<int> z(l, 0);
    int L = 0, R = 0;
    for (int i = 1; i < l; i++) {
        if (i > R) {
            L = R = i;
            while (R < l && concat[R - L] == concat[R]) R++;
            z[i] = R - L;
            R--;
        } else {
            int k = i - L;
            if (z[k] < R - i + 1) {
                z[i] = z[k];
            } else {
                L = i;
                while (R < l && concat[R - L] == concat[R]) R++;
                z[i] = R - L;
                R--;
            }
        }
    }
    vector<int> res;
    for (int i = 0; i < l; i++) {
        if (z[i] == pattern.length()) res.push_back(i - pattern.length() - 1);
    }
    return res;
}
```

## New Keywords / STL Used
- string concatenation

## Edge Cases
- character `$` appearing in text or pattern.

NEXT: [[Index]]
