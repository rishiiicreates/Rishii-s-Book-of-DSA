# Valid Sudoku Verification

## Problem Statement
- given a $9 \times 9$ Sudoku grid partially filled with digits `'1'`-`'9'` and empty cells `'.'`, mathematically verify if the current configuration is structurally valid.
- a configuration is valid if and only if:
- every row strictly contains unique digits.
- every column strictly contains unique digits.
- every $3 \times 3$ sub-box strictly contains unique digits.


## Approach: State Hashing via Bitmasking

- maintaining $27$ independent Hash Sets (9 rows, 9 columns, 9 sub-boxes) incurs severe memory allocation overhead and pointer chasing. Since the problem universe strictly consists of digits $1-9$, we can mathematically encode the presence of a digit $D$ as the $D$-th bit of a 32-bit integer.
- this reduces the state tracking to exactly $27$ scalar integers, enabling blazing-fast $\mathcal{O}(1)$ localized validation using [[Bitwise Operators]].

- for a digit $D$ at coordinate $(r, c)$:
- **digit Normalization:** Map $D \in ['1', '9']$ to integer $val \in [0, 8]$.
- **bitmask Generation:** Define $\text{mask} = 1 \ll val$.
- **sub-box Indexing Mapping:** The $9 \times 9$ grid decomposes into $9$ sub-boxes. The mathematical mapping from coordinate $(r, c)$ to the linearized sub-box index $B \in [0, 8]$ is strictly given by:
   $$ B = \left\lfloor \frac{r}{3} \right\rfloor \times 3 + \left\lfloor \frac{c}{3} \right\rfloor $$
- **collision Detection:** If $\text{rows}[r] \ \& \ \text{mask} \ne 0$, a collision has mathematically occurred. We apply the same check for $\text{cols}[c]$ and $\text{boxes}[B]$.
- **state Update:** If no collision is detected, structurally record the digit's presence via bitwise OR: $\text{rows}[r] \ |= \text{mask}$.


## Code Implementation

```cpp
#include <vector>

using namespace std;

bool isValidSudoku(vector<vector<char>>& board) {
    // Exactly 27 integer states, initialized to 0
    int rows[9] = {0};
    int cols[9] = {0};
    int boxes[9] = {0};

    for (int r = 0; r < 9; r++) {
        for (int c = 0; c < 9; c++) {
            if (board[r][c] == '.') continue;
            
            // Normalize digit and shift to create mask
            int val = board[r][c] - '1';
            int mask = 1 << val;
            
            // Map 2D coordinate to 1D sub-box index
            int box_idx = (r / 3) * 3 + (c / 3);
            
            // Mathematical collision detection via bitwise AND
            if ((rows[r] & mask) || (cols[c] & mask) || (boxes[box_idx] & mask)) {
                return false; // Structural invalidity detected
            }
            
            // State mutation via bitwise OR
            rows[r] |= mask;
            cols[c] |= mask;
            boxes[box_idx] |= mask;
        }
    }
    
    return true; // Complete structural validation
}
```

> [!important]
> Notice the usage of `int rows[9] = {0};` instead of `vector<int> rows(9, 0);`. Since the bounds are strictly fixed to $9$ by mathematical definition, stack-allocated raw arrays avoid dynamic heap allocations, further optimizing the theoretical space constraints to the bare minimum.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(1)$. The algorithm strictly processes exactly $81$ cells. The time bound is mathematically invariant.
- **space Complexity:** $\mathcal{O}(1)$. The algorithm allocates exactly $27$ integers (108 bytes), independent of any input variations.

NEXT: [[Index]]
