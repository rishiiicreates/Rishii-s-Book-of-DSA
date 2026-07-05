# Reverse Words in a String

## Problem Statement
- given an input string, reverse the order of the words. A word is defined as a sequence of non-space characters.

## Approach / Intuition
- first, reverse the entire [[String]]. Then, iterate through the string and use the [[Two-Pointer]] technique to find the boundaries of each word and reverse it individually. Finally, clean up any extra spaces that might have been present in the original string by shifting characters in-place.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(1)$ if done in-place

## Sample Code
```cpp
#include <string>
#include <algorithm>

using namespace std;

string reverseWords(string s) {
    reverse(s.begin(), s.end());
    int n = s.length(), storeIndex = 0;
    
    for (int i = 0; i < n; i++) {
        if (s[i] != ' ') {
            if (storeIndex != 0) s[storeIndex++] = ' ';
            int j = i;
            while (j < n && s[j] != ' ') s[storeIndex++] = s[j++];
            reverse(s.begin() + storeIndex - (j - i), s.begin() + storeIndex);
            i = j;
        }
    }
    s.erase(s.begin() + storeIndex, s.end());
    return s;
}
```

## New Keywords / STL Used
- `reverse`, `erase`

## Edge Cases
- multiple spaces between words, leading or trailing spaces, empty string.

NEXT: [[Index]]
