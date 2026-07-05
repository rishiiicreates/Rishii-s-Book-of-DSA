# Toggle Case

## Problem Statement
- given a string $S$, convert all lowercase letters to uppercase and all uppercase letters to lowercase. Leave non-alphabetical characters unchanged.

- *example:* $S = \text{"HeLlo! 123"}$ becomes $\text{"hElLO! 123"}$.


## Approach: ASCII Manipulation / Built-in Functions

- we can iterate through the string and check if a character is uppercase or lowercase.
- in C++, the standard library provides `<cctype>` functions like `islower()`, `isupper()`, `tolower()`, and `toupper()` which handle the [[ASCII Values]] math safely under the hood.


## Code Implementation

```cpp
#include <string>
#include <cctype>
using namespace std;

string toggleCase(string s) {
    for (char& c : s) {
        if (islower(c)) {
            c = toupper(c);
        } else if (isupper(c)) {
            c = tolower(c);
        }
    }
    return s;
}
```

> [!tip]
> By using `char& c` (passing by reference) in the range-based for loop, we are modifying the characters directly inside the original string `s` rather than modifying a copy of the character.

> [!important]
> If you were to implement the ASCII manipulation manually, you would check `if (c >= 'a' && c <= 'z')` and flip the case using `c = c - 32` (or `c ^= 32`). The bitwise XOR with $32$ works because the difference between uppercase and lowercase letters in ASCII is exactly $32$ (the 6th bit).


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the length of the string. We visit each character exactly once.
- **space Complexity:** $O(1)$ auxiliary space. The modification is done in-place inside the copied string $s$ (which is passed by value and returned).

NEXT: [[Index]]
