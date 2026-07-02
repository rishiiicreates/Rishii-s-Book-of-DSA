---
type: concept
tags: [hashing, cpp, strings]
date: 2026-06-30
---
# Valid Anagram

## Problem Statement
Given two strings $S$ and $T$, return `true` if $T$ is an anagram of $S$, and `false` otherwise. An anagram is mathematically defined as a permutation of the characters of a string.

*Example:* $S = \text{"anagram"}, T = \text{"nagaram"}$
*Result:* `true`

*Example:* $S = \text{"rat"}, T = \text{"car"}$
*Result:* `false`

---

## Approach: Frequency Hashing (Direct Addressing)

Since an anagram is a permutation, two strings are anagrams if and only if they possess identical character frequency distributions. 

Instead of an expensive sorting operation ($O(N \log N)$), we can use a Frequency Map. If the strings consist solely of lowercase English letters, we can bypass a generic [[Hash Map]] and use an array of size 26 for $O(1)$ Direct Addressing.
1. First, check if $|S| = |T|$. If not, they trivially cannot be anagrams.
2. Initialize an integer array `counts` of size 26 to zero.
3. Iterate $i$ from $0$ to $N-1$:
   - Increment the frequency for $S[i]$.
   - Decrement the frequency for $T[i]$.
4. If $S$ and $T$ are anagrams, every increment will be perfectly cancelled by a decrement. A final linear scan of the `counts` array verifying that $\forall i, \text{counts}[i] = 0$ confirms the anagram property.

---

## Code Implementation

```cpp
#include <string>
#include <vector>
using namespace std;

bool isAnagram(const string& s, const string& t) {
    // Lengths must match for a bijection to exist
    if (s.length() != t.length()) {
        return false;
    }
    
    // Frequency map using Direct Addressing for 26 lowercase English letters
    vector<int> counts(26, 0);
    
    // Single pass to increment for s and decrement for t
    for (int i = 0; i < s.length(); ++i) {
        counts[s[i] - 'a']++;
        counts[t[i] - 'a']--;
    }
    
    // Verify that the net frequency is strictly zero
    for (int count : counts) {
        if (count != 0) {
            return false;
        }
    }
    
    return true;
}
```

> [!warning]
> If the character set extends to Unicode (e.g., emojis or extended alphabets), the alphabet size is unbounded for an array. You must fall back to a standard `std::unordered_map<char, int>`.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N = |S| = |T|$. We perform a single pass over the strings and a constant 26-step verification loop.
- **Space Complexity:** $O(1)$ auxiliary space. The `counts` array is strictly bounded to 26 integers regardless of $N$.
