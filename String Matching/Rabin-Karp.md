---
type: concept
tags: [cpp, string_matching, hashing]
date: 2026-06-30
---
# Rabin-Karp

## Problem Statement
Given a text and a pattern, find all occurrences of the pattern in the text.

## Approach / Intuition
The [[Rabin-Karp Algorithm]] uses a [[Rolling Hash]] to quickly filter out positions in the text that cannot match the pattern. We only perform a full string comparison when the hash values match.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N + M)$ average, $O(N \times M)$ worst case
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> rabinKarp(string text, string pattern) {
    int n = text.length(), m = pattern.length();
    vector<int> res;
    if (m == 0 || n < m) return res;
    
    long long pHash = 0, tHash = 0, h = 1;
    long long d = 256, q = 1e9 + 7;
    
    for (int i = 0; i < m - 1; i++) h = (h * d) % q;
    for (int i = 0; i < m; i++) {
        pHash = (d * pHash + pattern[i]) % q;
        tHash = (d * tHash + text[i]) % q;
    }
    
    for (int i = 0; i <= n - m; i++) {
        if (pHash == tHash) {
            bool match = true;
            for (int j = 0; j < m; j++) {
                if (text[i + j] != pattern[j]) {
                    match = false;
                    break;
                }
            }
            if (match) res.push_back(i);
        }
        if (i < n - m) {
            tHash = (d * (tHash - text[i] * h) + text[i + m]) % q;
            if (tHash < 0) tHash += q;
        }
    }
    return res;
}
```

## New Keywords / STL Used
Modular arithmetic

## Edge Cases
Hash collisions, pattern length equal to text length.
