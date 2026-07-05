# Min Repeats for Substring

## Problem Statement
- given two strings $A$ and $B$, find the minimum number of times $A$ must be repeated such that $B$ is a substring of the repeated string.

## Approach / Intuition
- we can keep appending string $A$ to itself until its length is at least the length of string $B$. If $B$ is a [[Substring]] of this repeated string, we return the count. Otherwise, we append $A$ one more time and check again. If $B$ is still not found, it's impossible.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \times M)$ where $N$ and $M$ are string lengths
- **[[space Complexity]]:** $O(N + M)$

## Sample Code
```cpp
#include <string>

using namespace std;

int minRepeats(string A, string B) {
    string repeatedA = A;
    int count = 1;
    
    while (repeatedA.length() < B.length()) {
        repeatedA += A;
        count++;
    }
    
    if (repeatedA.find(B) != string::npos) {
        return count;
    }
    
    repeatedA += A;
    count++;
    
    if (repeatedA.find(B) != string::npos) {
        return count;
    }
    
    return -1;
}
```

## New Keywords / STL Used
- `std::string::find`, `std::string::npos`

## Edge Cases
- string $B$ has characters not present in $A$, string $A$ is empty, $B$ is already a substring of $A$.

NEXT: [[Index]]
