# Set Matrix Zeroes

## Problem Statement
- given an $M \times N$ integer matrix, if an element is `0`, you must mathematically project this zero state across its entire row and column.
- this must be done **in-place**.


## Approach: In-Place State Projection via Boundary Mapping

- a naive approach involves creating a mirrored $M \times N$ matrix to track zeroes, utilizing $\mathcal{O}(M \times N)$ space. A better approach uses two independent arrays of sizes $M$ and $N$ to track zeroes, dropping space to $\mathcal{O}(M + N)$.

- however, the absolute optimal approach enforces an $\mathcal{O}(1)$ space constraint by **hijacking the matrix's own boundaries (Row 0 and Col 0)** as the state-tracking vectors.
- to prevent data loss, we must first analyze the original states of Row 0 and Col 0:
- traverse Col 0: If any cell is 0, flag `firstColZero = true`.
- traverse Row 0: If any cell is 0, flag `firstRowZero = true`.

- now, we project the inner [[Matrix]] states onto the boundaries:
- iterate the subgrid ($i \in [1, M-1]$, $j \in [1, N-1]$). If $M[i][j] == 0$, set $M[i][0] = 0$ and $M[0][j] = 0$.

- next, we propagate the mapped states:
- re-iterate the subgrid. If either $M[i][0] == 0$ or $M[0][j] == 0$, set $M[i][j] = 0$.

- finally, we apply the initial flags:
- if `firstColZero` is true, zero out all of Col 0.
- if `firstRowZero` is true, zero out all of Row 0.


## Code Implementation

```cpp
#include <vector>
using namespace std;

void setZeroes(vector<vector<int>>& matrix) {
    if (matrix.empty()) return;
    
    int m = matrix.size();
    int n = matrix[0].size();
    bool firstRowZero = false;
    bool firstColZero = false;
    
    // Step 1: Detect states of the boundaries
    for (int j = 0; j < n; j++) {
        if (matrix[0][j] == 0) firstRowZero = true;
    }
    for (int i = 0; i < m; i++) {
        if (matrix[i][0] == 0) firstColZero = true;
    }
    
    // Step 2: Project inner zero states onto the boundaries
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0; // Mark row
                matrix[0][j] = 0; // Mark col
            }
        }
    }
    
    // Step 3: Consume the boundary states to nullify inner matrix
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }
    
    // Step 4: Finalize the boundaries
    if (firstColZero) {
        for (int i = 0; i < m; i++) matrix[i][0] = 0;
    }
    if (firstRowZero) {
        for (int j = 0; j < n; j++) matrix[0][j] = 0;
    }
}
```

> [!important]
> The order of operations is mathematically rigid. The inner matrix (Steps 2 and 3) must be fully processed **before** Step 4. Modifying Row 0 or Col 0 prematurely will contaminate the mapping states and catastrophically zero out incorrect sections of the matrix.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(M \times N)$. The algorithm strictly iterates over the matrix dimensions approximately twice, which boundedly scales as $\mathcal{O}(M \times N)$.
- **space Complexity:** $\mathcal{O}(1)$. The algorithm exploits existing matrix memory, utilizing exactly two boolean variables for auxiliary constraints.

NEXT: [[Index]]
