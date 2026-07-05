# Substrings with Exactly K Distinct Characters

## Problem Statement
- given a string of lowercase alphabets, count all possible substrings that contain exactly $K$ distinct characters.

## Approach / Intuition
- the problem of finding exactly $K$ distinct characters can be transformed into finding at most $K$ distinct characters minus at most $K-1$ distinct characters. Using the [[Sliding Window]] technique, we can implement a helper function `atMostK` that expands a window by adding characters to a frequency map and shrinks it from the left when the number of distinct characters exceeds $K$. The number of valid substrings ending at the current right pointer is `right - left + 1`.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(1)$ (fixed alphabet size 26)

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

int atMostK(string& s, int k) {
    if (k == 0) return 0;
    vector<int> count(26, 0);
    int distinct = 0, left = 0, res = 0;
    
    for (int right = 0; right < s.length(); right++) {
        if (count[s[right] - 'a'] == 0) distinct++;
        count[s[right] - 'a']++;
        
        while (distinct > k) {
            count[s[left] - 'a']--;
            if (count[s[left] - 'a'] == 0) distinct--;
            left++;
        }
        res += right - left + 1;
    }
    return res;
}

int exactK(string s, int k) {
    return atMostK(s, k) - atMostK(s, k - 1);
}
```

## New Keywords / STL Used
- frequency array optimization

## Edge Cases
- $k=0$, string length less than $K$, string with all identical characters.

NEXT: [[Index]]
