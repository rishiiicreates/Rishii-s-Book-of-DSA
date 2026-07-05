# Remove Occurrences

## Problem Statement
- given a string $S$ and a character $C$, remove all occurrences of $C$ from $S$ efficiently.

- *example:* $S = \text{"hello world"}$, $C = \text{'o'}$ $\rightarrow$ $S = \text{"hell wrld"}$.


## Approach: Erase-Remove Idiom (Optimal)

- while you could create a new string and only append characters that aren't $C$, that requires $O(N)$ extra space. In C++, the optimal way to filter elements from a string or vector in-place is the **Erase-Remove Idiom**.

- `std::remove` shifts all elements that are *not* $C$ to the front of the string, and returns an iterator to the new logical end of the string.
- `std::string::erase` actually resizes the string by deleting the "garbage" characters left at the end.


## Code Implementation

```cpp
#include <string>
#include <algorithm>
using namespace std;

string removeChar(string s, char ch) {
    s.erase(remove(s.begin(), s.end(), ch), s.end());
    return s;
}
```

> [!warning]
> `std::remove` does **not** actually change the size of the string or physically delete the elements. It merely overwrites elements. If you forget to call `.erase()`, your string will have the same original length but with residual garbage data at the tail!


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the string length. `std::remove` iterates through the sequence linearly.
- **space Complexity:** $O(1)$ auxiliary space. The characters are shifted in-place.

NEXT: [[Index]]
