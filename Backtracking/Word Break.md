# Word Break

## Problem Statement
- given a string and a dictionary of words, find all possible ways to break the string into valid dictionary words.

## Approach / Intuition
- use [[Backtracking]] to try splitting the [[String]] at every possible prefix that exists in the dictionary. Upon finding a valid prefix, recurse on the suffix. To optimize and avoid redundant computations, we can combine this with memoization (part of [[Dynamic Programming]]).

## Time & Space Complexity
- **[[time Complexity]]:** $O(2^N)$ in worst case for all paths
- **[[space Complexity]]:** $O(N)$ for recursion stack (plus output storage)

## Sample Code
```cpp
#include <string>
#include <vector>
#include <unordered_set>

using namespace std;

void backtrack(string s, unordered_set<string>& dict, string current, vector<string>& res) {
    if (s.empty()) {
        res.push_back(current.substr(0, current.length() - 1));
        return;
    }
    for (int i = 1; i <= s.length(); i++) {
        string prefix = s.substr(0, i);
        if (dict.count(prefix)) {
            backtrack(s.substr(i), dict, current + prefix + " ", res);
        }
    }
}

vector<string> wordBreak(int n, vector<string>& dict, string s) {
    unordered_set<string> wordSet(dict.begin(), dict.end());
    vector<string> res;
    backtrack(s, wordSet, "", res);
    return res;
}
```

## New Keywords / STL Used
- `unordered_set`, `substr`

## Edge Cases
- no valid breakdown exists, string matches exactly one word in the dictionary, multiple overlapping dictionary words.

NEXT: [[Index]]
