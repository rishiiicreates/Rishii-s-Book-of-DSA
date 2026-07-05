# Pangram

## Problem Statement
- determine if a string is a pangram. A string is mathematically defined as a pangram if it contains every letter of the English alphabet (a-z or A-Z) at least once.


## Approach: Boolean Array vs. Bitmask

- the goal is to track the presence of 26 independent boolean states (the letters 'a' through 'z').

### Strategy 1: Boolean Vector (Hash Set analog)
- we map each alphabetic character to an integer index $0 \le \text{idx} < 26$. We maintain a boolean array `seen` and an accumulator `count`. As we traverse the string, if we encounter an unvisited character, we mark it `true` and increment `count`. If `count` reaches 26, the string is a pangram.

### Strategy 2: Bitmask (Advanced & Optimal)
- instead of allocating a vector, we can encode the 26 states into a single 32-bit integer!
- setting the $i$-th bit of an integer `mask` tracks the presence of the $i$-th letter.
- at the end, if the mask equals $(1 \ll 26) - 1$, it means the first 26 bits are all `1`, proving it's a pangram.


## Code Implementation

### Boolean Vector Approach

```cpp
#include <string>
#include <vector>
#include <cctype>
using namespace std;

bool isPangram(string s) {
    if (s.length() < 26) return false; // Mathematical impossibility
    
    vector<bool> seen(26, false);
    int count = 0;
    
    for (char c : s) {
        if (isalpha(c)) {
            int idx = tolower(c) - 'a';
            if (!seen[idx]) {
                seen[idx] = true;
                count++;
            }
        }
        if (count == 26) return true; // Early exit
    }
    
    return false;
}
```

### Bitmask Approach (Elite)

```cpp
#include <string>
#include <cctype>
using namespace std;

bool isPangramBitmask(string s) {
    if (s.length() < 26) return false;
    
    int mask = 0;
    for (char c : s) {
        if (isalpha(c)) {
            mask |= (1 << (tolower(c) - 'a'));
        }
        // (1 << 26) - 1 is exactly 26 bits of 1s (0x3FFFFFF)
        if (mask == (1 << 26) - 1) return true; 
    }
    return false;
}
```

> [!important]
> The bitwise approach strictly requires $\mathcal{O}(1)$ space utilizing a single standard 32-bit integer. The check `(1 << 26) - 1` utilizes integer overflow properties to generate exactly 26 continuous 1-bits safely.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. We traverse the string of size $N$ at most once. The early exit guarantees minimal execution time for valid pangrams.
- **space Complexity:** $\mathcal{O}(1)$. Both the boolean vector of size 26 and the 32-bit integer mask require constant memory regardless of the string's immense size.

NEXT: [[Index]]
