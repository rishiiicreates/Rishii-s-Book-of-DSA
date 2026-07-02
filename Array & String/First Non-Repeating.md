---
type: concept
tags: [string, cpp, hash-map, counting]
date: 2026-06-30
---
# First Non-Repeating Character

## Problem Statement
Find the first non-repeating character in a string and return it. If no such character exists, return a null character `\0` (or a specific error flag/index depending on the signature).

---

## Approach: Two-Pass Frequency Array

To solve this efficiently, we leverage a frequency map. The mathematical transformation involves mapping each character's ASCII value to an index in a fixed-size array. 

This is a quintessential **Two-Pass Algorithm**:
1. **Pass 1 (Counting):** Iterate through the string. For every character $c$, increment its frequency at the mapped index in our array.
2. **Pass 2 (Verification):** Iterate through the string *again, from the beginning*. For every character $c$, check its frequency in the map. The first character encountered whose mapped frequency is exactly $1$ is guaranteed to be the first non-repeating character in the string.

Using a fixed-size array (`256` for ASCII) acts as a perfect spatial hash map guaranteeing $\mathcal{O}(1)$ access and insertion time, without the overhead of dynamic hashing mechanisms like `std::unordered_map`.

---

## Code Implementation

```cpp
#include <string>
#include <vector>
using namespace std;

char firstNonRepeating(string s) {
    // 256 covers the entire extended ASCII set
    vector<int> count(256, 0);
    
    // First Pass: Build the frequency histogram
    for (char c : s) {
        count[(unsigned char)c]++;
    }
    
    // Second Pass: Find the first character with frequency exactly 1
    for (char c : s) {
        if (count[(unsigned char)c] == 1) {
            return c;
        }
    }
    
    // Return null character if all characters repeat or string is empty
    return '\0';
}
```

> [!tip]
> We cast `c` to `unsigned char` before using it as an array index. Standard `char` can be signed in C++, leading to negative indices (and subsequent memory corruption/segmentation faults) if non-standard ASCII characters are passed.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. We iterate through the string of length $N$ exactly twice. Thus, the time is strictly $2N$, which bounds to $\mathcal{O}(N)$.
- **Space Complexity:** $\mathcal{O}(1)$. The `count` array strictly requires a fixed 256 integers of memory, which does not scale with respect to the input string length $N$.
