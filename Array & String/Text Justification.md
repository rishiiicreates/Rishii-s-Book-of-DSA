# Text Justification

## Problem Statement
- given an array of string `words` and an integer `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

- mathematical Constraints:
- pack as many words as possible on each line (Greedy packing).
- distribute spaces evenly. If uneven, the leftmost space slots absorb the remainder.
- the final line must be left-justified with zero extra internal spacing.


## Approach: Greedy Packing & Modulo Distribution

- this problem is a masterclass in index simulation and deterministic state constraints.

### 1. Greedy Packing
- for a starting word at index `i`, we greedily expand an index `j` until the cumulative length of the words plus the mandatory minimum spaces (`j - i - 1`) exceeds `maxWidth`. The block of words constrained by $[i, j-1]$ strictly defines our current line.

### 2. Space Distribution Mathematics
- let $W$ be the number of words in the line (`j - i`), and $L$ be the sum of their raw character lengths.
- the total available spaces $S = \text{maxWidth} - L$.
- the number of interconnecting gaps $G = W - 1$.

- if $G == 0$ (a single word on the line), or if this is the **last line** ($j == N$):
- the line must be left-justified. Words are separated by exactly 1 space, and the tail is heavily padded with spaces until `maxWidth` is reached.

- otherwise, for standard lines:
- base spaces per gap: $B = \lfloor S / G \rfloor$
- remainder spaces (distributed left-to-right): $R = S \pmod G$

- a gap receives $B + 1$ spaces if its relative index is strictly less than $R$. Otherwise, it receives $B$ spaces.


## Code Implementation

```cpp
#include <vector>
#include <string>
using namespace std;

vector<string> fullJustify(vector<string>& words, int maxWidth) {
    vector<string> res;
    int n = words.size();
    int i = 0;
    
    while (i < n) {
        int j = i + 1;
        int lineLength = words[i].length();
        
        // Greedy Packing Phase
        while (j < n && lineLength + words[j].length() + (j - i) <= maxWidth) {
            lineLength += words[j].length();
            j++;
        }
        
        // Mathematical Space Configuration
        int diff = maxWidth - lineLength;
        int numberOfWords = j - i;
        
        // Edge Case: Last line or Single-word line
        if (numberOfWords == 1 || j >= n) {
            string line = words[i];
            for (int k = i + 1; k < j; k++) {
                line += " " + words[k];
            }
            // Pad right side
            line += string(maxWidth - line.length(), ' ');
            res.push_back(line);
        } 
        // Standard Fully Justified Line
        else {
            int spaces = diff / (numberOfWords - 1);
            int extraSpaces = diff % (numberOfWords - 1);
            
            string line = words[i];
            for (int k = i + 1; k < j; k++) {
                // Apply Modulo Remainder Spaces
                int spacesToApply = spaces + (extraSpaces > 0 ? 1 : 0);
                extraSpaces--;
                
                line += string(spacesToApply, ' ') + words[k];
            }
            res.push_back(line);
        }
        
        // Shift boundary
        i = j;
    }
    
    return res;
}
```

> [!warning]
> Be extremely cautious with the string constructor `string(size_t n, char c)` in C++. Using it appropriately dynamically generates the exact number of consecutive spaces without resorting to inefficient `while` loops. 


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(L)$, where $L$ is the total number of characters across all words in the array. Every character is processed and strictly concatenated once.
- **space Complexity:** $\mathcal{O}(L)$ to construct the final array of justified string lines.

NEXT: [[Index]]
