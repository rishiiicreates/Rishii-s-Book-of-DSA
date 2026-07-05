# String

## Problem Statement
- understand string manipulation, which represents a sequence of characters, and the common operations provided by the C++ standard library.


## Approach: Array of Characters

- a **String** is essentially a one-dimensional array of characters. In C++, `std::string` provides a dynamic, memory-managed container for text. It abstracts away the manual memory management of C-style character arrays (`char[]`) and provides high-level operations like concatenation, substring extraction, and lexicographical comparison.

- under the hood, a string is stored in contiguous memory, meaning operations like indexed access are $O(1)$. However, operations that modify the length of the string, such as concatenation, may require allocating a new, larger memory block and copying the existing characters.

### String Immutability
- unlike strings in Java or Python, strings in C++ are **mutable**. You can directly change a character at a specific index in $O(1)$ time, e.g., `s[i] = 'a'`.


## Code Implementation

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    string s = "hello";
    
    // Concatenation: Amortized O(M) where M is the length of the appended string
    s += " world"; 
    
    // Substring extraction: O(K) where K is the length of the substring
    string sub = s.substr(0, 5); // Extracts "hello"
    
    // Mutable access: O(1)
    s[0] = 'H';
    
    cout << s << "\n";
    return 0;
}
```


## Complexity Analysis
- **time Complexity:**
  - access/Mutation: $O(1)$ by index.
  - concatenation: $O(K)$ where $K$ is the length of the string being appended.
  - substring (`substr`): $O(K)$ where $K$ is the length of the substring to extract.
- **space Complexity:** $O(N)$ to store a string of length $N$.

> [!important]
> When passing strings to functions in C++, always pass them by constant reference (`const std::string&`) unless you need a copy. Passing by value triggers a full $O(N)$ copy of the string memory.

NEXT: [[Index]]
