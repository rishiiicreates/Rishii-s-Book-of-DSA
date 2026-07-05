# Smallest Window Containing All Characters of Another String

## Problem Statement
- given two strings $S$ and $P$, find the smallest substring in $S$ containing all characters of $P$ (including duplicates).

## Approach / Intuition
- use a frequency map to store the required counts of characters from $P$. Utilize a [[Sliding Window]] on $S$. As the `right` pointer moves, decrement the required counts and track how many needed characters have been matched. When all characters are matched, shrink the window from the `left` to find the minimum length, saving the best starting index and length.

## Time & Space Complexity
- **[[time Complexity]]:** $O(|S| + |P|)$
- **[[space Complexity]]:** $O(1)$ (constant size map for characters)

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

string minWindow(string s, string p) {
    vector<int> map(256, 0);
    for (char c : p) map[c]++;
    
    int counter = p.length(), begin = 0, end = 0, d = 1e9, head = 0;
    while (end < s.length()) {
        if (map[s[end++]]-- > 0) counter--;
        
        while (counter == 0) {
            if (end - begin < d) d = end - (head = begin);
            if (map[s[begin++]]++ == 0) counter++;
        }
    }
    return d == 1e9 ? "" : s.substr(head, d);
}
```

## New Keywords / STL Used
- `substr`

## Edge Cases
- $p$ longer than $S$, no such window exists, $S$ and $P$ are identical.

NEXT: [[Index]]
