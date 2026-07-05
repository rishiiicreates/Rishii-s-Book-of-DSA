# Same Strings

## Problem Statement
- given two strings $S_1$ and $S_2$, determine if they are exactly identical in terms of characters and order.

- *example:* $S_1 = \text{"hello"}$, $S_2 = \text{"hello"}$ returns `true`. $S_1 = \text{"Hello"}$, $S_2 = \text{"hello"}$ returns `false`.


## Approach: Lexicographical Comparison

- the simplest and most optimal way to compare two strings in C++ is by using the built-in equality operator `==`. This operator is overloaded for `std::string` to perform a lexicographical comparison.

- it first checks if the lengths of both strings are equal. If not, they can't be identical.
- if the lengths match, it compares the characters one by one.


## Code Implementation

```cpp
#include <string>
using namespace std;

bool areSame(const string& s1, const string& s2) {
    // The == operator handles length checks and character comparisons
    return s1 == s2;
}
```

> [!tip]
> Notice the use of `const string&`. Passing strings by constant reference avoids making a deep copy of the strings, making the function significantly faster and more memory-efficient.

> [!important]
> The equality operator is case-sensitive. It relies on the [[ASCII Values]] of characters, so `"A"` and `"a"` are treated as different.


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the length of the string. In the worst case (where all characters match up to the end), it must check all characters. If lengths differ, it takes $O(1)$.
- **space Complexity:** $O(1)$ as we only keep references to the original strings and use no extra space.

NEXT: [[Index]]
