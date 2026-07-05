# Conway's Game Of Life

## Problem Statement
- given an $M \times N$ grid where each cell is either `0` (dead) or `1` (live), compute the next state of the grid according to the rules of Conway's Game of Life. The updates must be performed simultaneously (i.e., in-place) without allocating a separate $O(M \times N)$ board.
- the rules for transitions are:
- any live cell with fewer than two live neighbors dies (underpopulation).
- any live cell with two or three live neighbors lives on.
- any live cell with more than three live neighbors dies (overpopulation).
- any dead cell with exactly three live neighbors becomes a live cell (reproduction).


## Approach: State Machine Encoding

- if we overwrite a cell directly, we destroy its original state, corrupting the neighbor calculations for subsequent cells. To achieve simultaneous updates in an $O(1)$ auxiliary space constraint, we use a **State Machine Encoding**.

- since the grid only uses bits `0` and `1`, we can encode the *transition* states using higher integers.
- let's define the state mapping mathematically:
- `0` $\rightarrow$ Dead (remains dead)
- `1` $\rightarrow$ Live (remains live)
- `2` $\rightarrow$ Was Live, now Dead
- `3` $\rightarrow$ Was Dead, now Live

- when checking the current state of a cell for neighbor counting, a cell is considered "currently live" if its value is `1` OR `2`.
- after fully iterating the grid and applying these intermediate states, we perform a second pass to resolve the states: `2` becomes `0`, and `3` becomes `1`.


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

void gameOfLife(vector<vector<int>>& board) {
    int m = board.size();
    if (m == 0) return;
    int n = board[0].size();
    
    // Direction vectors for all 8 neighbors
    int dr[] = {-1, -1, -1, 0, 0, 1, 1, 1};
    int dc[] = {-1, 0, 1, -1, 1, -1, 0, 1};
    
    // Pass 1: Encode intermediate states
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            int liveNeighbors = 0;
            
            // Count live neighbors
            for (int k = 0; k < 8; k++) {
                int r = i + dr[k];
                int c = j + dc[k];
                // A cell was originally live if it's 1 (remains live) or 2 (becomes dead)
                if (r >= 0 && r < m && c >= 0 && c < n && (board[r][c] == 1 || board[r][c] == 2)) {
                    liveNeighbors++;
                }
            }
            
            // Apply Conway's Rules using state transitions
            if (board[i][j] == 1 && (liveNeighbors < 2 || liveNeighbors > 3)) {
                board[i][j] = 2; // Rule 1 & 3: Live to Dead
            }
            if (board[i][j] == 0 && liveNeighbors == 3) {
                board[i][j] = 3; // Rule 4: Dead to Live
            }
        }
    }
    
    // Pass 2: Resolve intermediate states
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 2) board[i][j] = 0;
            else if (board[i][j] == 3) board[i][j] = 1;
        }
    }
}
```


## Complexity Analysis
- **time Complexity:** $O(M \times N)$. For each of the $M \times N$ cells, we do exactly $8$ neighbor checks. This is strictly linear with respect to the grid size.
- **space Complexity:** $O(1)$ auxiliary space. The grid is modified in-place using our bit-level / integer state encoding.

> [!important]
> **Infinite Grids:** If the grid is infinite (or extremely sparse and large), storing the full matrix is inefficient. Instead, store only the live cells in a hash set of coordinates. To calculate the next state, iterate only over the live cells and their immediate neighbors.

NEXT: [[Index]]
