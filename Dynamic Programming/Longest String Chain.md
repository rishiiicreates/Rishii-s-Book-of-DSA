# Longest String Chain

## Problem Statement
- given a list of words, find the length of the longest string chain where each word adds exactly one letter to the previous word.

## Approach / Intuition
- sort the words by length. This ensures we process shorter words first. Use a [[Hash Map]] to store the longest chain ending at each word. For each word, generate all possible predecessors by removing one character at a time. The chain length for the current word is `1 + max(chain lengths of predecessors)`.

## Time & Space Complexity
- **[[time Complexity]]:** O(N \log N + N \times L^2), where L is the maximum word length
- **[[space Complexity]]:** O(N \times L)

## Sample Code
```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>

using namespace std;

int longestStrChain(vector<string>& words) {
    sort(words.begin(), words.end(), [](const string& a, const string& b) {
        return a.length() < b.length();
    });
    
    unordered_map<string, int> dp;
    int max_chain = 1;
    
    for (const string& word : words) {
        int current_chain = 1;
        for (int i = 0; i < word.length(); ++i) {
            string predecessor = word.substr(0, i) + word.substr(i + 1);
            if (dp.count(predecessor)) {
                current_chain = max(current_chain, dp[predecessor] + 1);
            }
        }
        dp[word] = current_chain;
        max_chain = max(max_chain, current_chain);
    }
    
    return max_chain;
}
```

## New Keywords / STL Used
- `std::sort` with custom lambda, `std::unordered_map`, `std::string::substr`

## Edge Cases
- all words of same length, single word array, large number of words but no valid chains.

NEXT: [[Index]]
