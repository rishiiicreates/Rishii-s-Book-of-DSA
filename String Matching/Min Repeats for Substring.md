---
type: concept
tags: [cpp, string_matching]
date: 2026-06-30
---
# Min Repeats for Substring

## Problem Statement
Given two strings $A$ and $B$, find the minimum number of times $A$ must be repeated such that $B$ is a substring of the repeated string.

## Approach / Intuition
We can keep appending string $A$ to itself until its length is at least the length of string $B$. If $B$ is a [[Substring]] of this repeated string, we return the count. Otherwise, we append $A$ one more time and check again. If $B$ is still not found, it's impossible.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \times M)$ where $N$ and $M$ are string lengths
- **[[Space Complexity]]:** $O(N + M)$

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
`std::string::find`, `std::string::npos`

## Edge Cases
String $B$ has characters not present in $A$, string $A$ is empty, $B$ is already a substring of $A$.
