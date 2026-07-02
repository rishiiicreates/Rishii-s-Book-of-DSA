---
type: concept
tags: [hashing, cpp, math, bijection]
date: 2026-07-01
---
# Isomorphic Strings

## Problem Statement
Given two strings $S$ and $T$, mathematically determine if they form an exact structural isomorphism. Two strings are isomorphic if there exists a strict bijection $f: \Sigma_S \to \Sigma_T$ such that mapping characters in $S$ yields $T$, preserving total topological order.

---

## Approach: Structural Fingerprinting via Indexing

A pure bijective mapping requires ensuring both injectivity (no two characters in $S$ map to the same character in $T$) and surjectivity (every character in $T$ maps back to $S$). Using two separate hash maps to track the mappings $S \to T$ and $T \to S$ is standard but operationally heavy.

Instead, we extract the structural topology of the strings using an **Index Tracking State Machine**.
We utilize two parallel flat arrays (acting as $O(1)$ hash maps) to track the *most recent sequential index* where each character was observed.
For the strings to be isomorphic, at every step $i$, the last seen topological index of $S[i]$ must perfectly match the last seen topological index of $T[i]$.

If a divergence is detected in the index history, the bijective assumption is fundamentally shattered.
At each step, we update both history arrays with the current index $i$.

---

## Code Implementation

```cpp
#include <string>
#include <vector>

using namespace std;

bool isIsomorphic(string s, string t) {
    if (s.length() != t.length()) return false;
    
    // ASCII mapped flat arrays initialized to -1
    // Space is strictly bounded to 256 for standard ASCII
    vector<int> history_s(256, -1);
    vector<int> history_t(256, -1);
    
    for (int i = 0; i < s.length(); i++) {
        // Topological divergence breaks the bijection
        if (history_s[s[i]] != history_t[t[i]]) {
            return false;
        }
        
        // Update topological footprint
        history_s[s[i]] = i;
        history_t[t[i]] = i;
    }
    
    return true;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$, where $N$ is the length of the strings. The flat array indices operate in strict $O(1)$ constant time per iteration.
- **Space Complexity:** $O(1)$ auxiliary space. The arrays are rigidly bound to size $256$, invariant to $N$.

> [!important]
> **ASCII Range Constraints:** The algorithm relies on the ASCII cardinality (256). If the strings are encoded in UTF-8 or contain extended Unicode glyphs, flat array allocations become mathematically intractable. In such domains, `std::unordered_map<char32_t, int>` must replace the flat vectors to handle sparse arbitrary codepoints.
