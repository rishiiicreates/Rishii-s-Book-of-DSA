---
type: concept
tags: [array, string, cpp, string-manipulation, two-pointers]
date: 2026-06-30
---
# URLify

## Problem Statement
Given a string and its "true" length, write a method to replace all spaces in the string with `%20`. You may assume that the string has sufficient space at the end to hold the additional characters.

---

## Approach: Backwards Two-Pointer Overwrite

A naive approach involves shifting all characters to the right by 2 positions every time a space is encountered, leading to an abysmal $\mathcal{O}(N^2)$ time complexity due to repeated cascading shifts.

To achieve $\mathcal{O}(N)$ time and $\mathcal{O}(1)$ space, we must modify the string **in-place**.
The mathematical trick for in-place string expansion is to **iterate backwards**.
1. First, count the number of spaces in the "true" portion of the string to calculate the exact final length of the string: `final_length = true_length + spaces * 2`.
2. Use two pointers: one pointing to the end of the true string (`i = true_length - 1`), and one pointing to the end of the newly calculated final string (`index = final_length - 1`).
3. Iterate `i` backwards. If `str[i]` is a space, we write `%20` at the `index` and decrement `index` by 3. Otherwise, we copy `str[i]` to `str[index]` and decrement `index` by 1.

By moving backwards, we mathematically guarantee that we will never overwrite a character before it has been processed, as the `index` pointer will always be ahead of or equal to the `i` pointer.

---

## Code Implementation

```cpp
#include <string>
using namespace std;

void replaceSpaces(string& str, int trueLength) {
    int spaceCount = 0;
    
    // Step 1: Count the spaces within the true length
    for (int i = 0; i < trueLength; i++) {
        if (str[i] == ' ') {
            spaceCount++;
        }
    }
    
    // Step 2: Calculate the exact boundary of the new string
    int index = trueLength + spaceCount * 2;
    
    // Explicitly null-terminate the extended string if required by the environment
    if (index < str.length()) {
        str[index] = '\0';
    }
    
    // Step 3: Traverse backwards and expand
    for (int i = trueLength - 1; i >= 0; i--) {
        if (str[i] == ' ') {
            str[index - 1] = '0';
            str[index - 2] = '2';
            str[index - 3] = '%';
            index -= 3;
        } else {
            str[index - 1] = str[i];
            index--;
        }
    }
}
```

> [!warning]
> In C++, `std::string` manages its own length dynamically. This specific algorithm is historically designed for C-style character arrays (`char[]`). When adapting it for `std::string`, ensure the string has been pre-allocated (e.g., via `str.resize()`) with enough trailing padding to support the shift.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$, where $N$ is the true length of the string. We make exactly two passes: one to count spaces, and one to shift characters.
- **Space Complexity:** $\mathcal{O}(1)$. The string is modified strictly in-place with zero auxiliary data structures.
