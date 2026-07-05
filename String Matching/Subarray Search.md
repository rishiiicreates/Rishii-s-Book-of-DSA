# Subarray Search

## Problem Statement
- find if a subarray pattern exists within a given array (similar to string matching but for arrays).

## Approach / Intuition
- we can treat the array as a string and use [[KMP Algorithm]] or [[Rabin-Karp Algorithm]]. In KMP, we build the LPS array for the pattern array and perform linear traversal over the given array to find matches.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N + M)$
- **[[space Complexity]]:** $O(M)$

## Sample Code
```cpp
#include <vector>

using namespace std;

int findSubarray(const vector<int>& arr, const vector<int>& pattern) {
    int n = arr.size();
    int m = pattern.size();
    if (m == 0) return 0;
    if (n < m) return -1;
    
    vector<int> lps(m, 0);
    int len = 0;
    for (int i = 1; i < m; ) {
        if (pattern[i] == pattern[len]) {
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
    
    int i = 0, j = 0;
    while (i < n) {
        if (arr[i] == pattern[j]) {
            i++; j++;
        }
        if (j == m) {
            return i - j;
        } else if (i < n && arr[i] != pattern[j]) {
            if (j != 0) j = lps[j - 1];
            else i++;
        }
    }
    return -1;
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- empty pattern, array smaller than pattern, pattern not found.

NEXT: [[Index]]
