# Palindromic Partitions

## Problem Statement
- given a string, find all possible partitions such that every substring of the partition is a palindrome.

## Approach / Intuition
- use [[Backtracking]] to generate partitions. Iterate through the [[String]], slicing prefixes. If a prefix is a palindrome, recursively partition the remaining suffix. Keep track of the current sequence of palindromes, and when the end of the string is reached, add the sequence to the result list.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \cdot 2^N)$
- **[[space Complexity]]:** $O(N)$ for recursion stack

## Sample Code
```cpp
#include <vector>
#include <string>

using namespace std;

bool isPalindrome(string& s, int start, int end) {
    while (start <= end) {
        if (s[start++] != s[end--]) return false;
    }
    return true;
}

void solve(int index, string& s, vector<string>& path, vector<vector<string>>& res) {
    if (index == s.length()) {
        res.push_back(path);
        return;
    }
    
    for (int i = index; i < s.length(); i++) {
        if (isPalindrome(s, index, i)) {
            path.push_back(s.substr(index, i - index + 1));
            solve(i + 1, s, path, res);
            path.pop_back();
        }
    }
}

vector<vector<string>> partition(string s) {
    vector<vector<string>> res;
    vector<string> path;
    solve(0, s, path, res);
    return res;
}
```

## New Keywords / STL Used
- `pop_back`, `substr`

## Edge Cases
- empty string, string with all identical characters, string with no palindromic substrings longer than 1.

NEXT: [[Index]]
