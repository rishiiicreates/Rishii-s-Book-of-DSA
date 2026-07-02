---
type: concept
tags: [string, cpp, stl]
date: 2026-06-30
---
# Remove Spaces

## Problem Statement
Given a string $S$, remove all whitespace characters efficiently.

*Example:* $S = \text{"  a  b   c "}$ $\rightarrow$ $S = \text{"abc"}$.

---

## Approach: Erase-Remove Idiom

This is a direct application of the logic from [[Remove Occurrences]]. We target the space character (`' '`) using the **Erase-Remove Idiom** to efficiently compact the string.

1. `std::remove` shifts all non-space characters to the front.
2. `std::string::erase` trims the leftover garbage characters from the new logical end to the physical end.

---

## Code Implementation

```cpp
#include <string>
#include <algorithm>
using namespace std;

string removeSpaces(string s) {
    s.erase(remove(s.begin(), s.end(), ' '), s.end());
    return s;
}
```

> [!tip]
> If you need to remove *all* types of whitespace (including tabs `\t`, newlines `\n`, etc.) rather than just the space character, you can use `std::remove_if` paired with `::isspace`.
> ```cpp
> s.erase(remove_if(s.begin(), s.end(), ::isspace), s.end());
> ```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the string length. The shifting operation checks every character linearly.
- **Space Complexity:** $O(1)$ auxiliary space. The modification is strictly in-place.
