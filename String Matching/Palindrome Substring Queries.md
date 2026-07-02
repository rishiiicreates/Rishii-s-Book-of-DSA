---
type: concept
tags: [cpp, string_matching, hashing]
date: 2026-06-30
---
# Palindrome Substring Queries

## Problem Statement
Answer queries asking whether a given substring $S[L..R]$ is a palindrome.

## Approach / Intuition
By using a [[Rolling Hash]] algorithm, we can compute the forward hash and reverse hash of any substring in $O(1)$ time after $O(N)$ preprocessing. A substring is a palindrome if its forward hash equals its reverse hash.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$ for preprocessing, $O(1)$ per query
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

class PalindromeQuery {
    vector<long long> forward, reverse, p;
    long long mod = 1e9 + 7;
    int base = 31;
    int n;

public:
    PalindromeQuery(string s) {
        n = s.length();
        forward.assign(n + 1, 0);
        reverse.assign(n + 1, 0);
        p.assign(n + 1, 1);

        for (int i = 0; i < n; i++) {
            p[i + 1] = (p[i] * base) % mod;
            forward[i + 1] = (forward[i] * base + (s[i] - 'a' + 1)) % mod;
            reverse[i + 1] = (reverse[i] * base + (s[n - 1 - i] - 'a' + 1)) % mod;
        }
    }

    bool isPalindrome(int l, int r) {
        long long hashF = (forward[r + 1] - forward[l] * p[r - l + 1]) % mod;
        if (hashF < 0) hashF += mod;

        int revL = n - 1 - r;
        int revR = n - 1 - l;
        long long hashR = (reverse[revR + 1] - reverse[revL] * p[revR - revL + 1]) % mod;
        if (hashR < 0) hashR += mod;

        return hashF == hashR;
    }
};
```

## New Keywords / STL Used
`std::vector::assign`

## Edge Cases
Single character query, full string query, out of bounds indices.
