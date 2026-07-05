# Longest Substring with At Most K Distinct Characters

## Problem Statement
- given a string, find the length of the longest substring that contains at most `k` distinct characters.

## Approach / Intuition
- maintain a frequency map and a counter for distinct characters within a [[Sliding Window]]. Expand the window by advancing `right`. When the distinct character count exceeds `k`, advance `left` and update the frequency map until the distinct count drops back to `k`. Keep track of the maximum window size.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1) or O(K) for character map

## Sample Code
```cpp
int lengthOfLongestSubstringKDistinct(string s, int k) {
    if (k == 0) return 0;
    unordered_map<char, int> freq;
    int maxLen = 0, left = 0;
    for (int right = 0; right < s.length(); ++right) {
        freq[s[right]]++;
        while (freq.size() > k) {
            freq[s[left]]--;
            if (freq[s[left]] == 0) {
                freq.erase(s[left]);
            }
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## New Keywords / STL Used
- `unordered_map::erase`

## Edge Cases
- `k = 0`, empty string, `k` greater than string length, string with fewer than `k` distinct characters.

NEXT: [[Index]]
