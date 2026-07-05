# Non-Attacking Knights

## Problem Statement
- given an $N \times N$ chessboard, place $K$ knights such that no two knights attack each other.
- find the total number of distinct ways to place them.


## Approach: Backtracking

- this is a classic constraint-satisfaction problem that cannot be solved with a pure mathematical formula; we must explore the state space.
- we use **Backtracking**, which is an advanced form of recursion:
- **choose:** Pick a square on the board. Check if it's safe to place a knight there.
- **explore:** If safe, place the knight and recursively move to the next square to place the remaining $K-1$ knights.
- **un-choose (Backtrack):** Remove the knight from the square, and recursively move to the next square assuming we *didn't* place a knight there.

### The Knight's Attack Logic
- a knight in chess attacks in an "L" shape. From `(r, c)`, it attacks 8 possible squares:
- `(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)`.
- before placing a knight, we must loop through these 8 offsets and ensure no other knight currently exists there.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

// Check if cell (r,c) is attacked by any existing knight
bool isSafe(int r, int c, vector<vector<int>>& board, int n) {
    int dr[] = {-2, -2, -1, -1, 1, 1, 2, 2};
    int dc[] = {-1, 1, -2, 2, -2, 2, -1, 1};
    
    for (int i = 0; i < 8; i++) {
        int nr = r + dr[i];
        int nc = c + dc[i];
        // If inside the board AND a knight exists there, it's unsafe
        if (nr >= 0 && nr < n && nc >= 0 && nc < n && board[nr][nc] == 1) {
            return false;
        }
    }
    return true;
}

void solve(int n, int k, int r, int c, vector<vector<int>>& board, int& count) {
    // Base Case 1: All K knights placed successfully
    if (k == 0) {
        count++;
        return;
    }
    
    // Base Case 2: Walked off the board
    if (r >= n) return;
    
    // Calculate the coordinates of the next cell (wrapping around rows)
    int next_c = (c + 1) % n;
    int next_r = r + (c + 1) / n;
    
    // Branch 1: Try placing the knight here (if safe)
    if (isSafe(r, c, board, n)) {
        board[r][c] = 1;                                      // Choose
        solve(n, k - 1, next_r, next_c, board, count);        // Explore
        board[r][c] = 0;                                      // Un-choose (Backtrack)
    }
    
    // Branch 2: Don't place a knight here, just move to the next cell
    solve(n, k, next_r, next_c, board, count);
}
```

> [!tip] 
> Notice how we calculate `next_c` and `next_r`. This is a brilliant trick to traverse a 2D matrix using a purely 1D sequence. `c + 1` moves right. Modulo `n` wraps it back to column `0`. Division by `n` adds `1` to the row only when the column wraps around!


## Complexity Analysis
- **time Complexity:** $O\left(\binom{N^2}{K}\right)$. We are essentially choosing $K$ squares out of $N^2$ squares. This represents a massive decision tree. For large $N$ and $K$, this approach will Time Limit Exceed (TLE).
- **space Complexity:** $O(N^2)$ to store the chessboard matrix, plus $O(N^2)$ for the maximum depth of the recursive Call Stack.

NEXT: [[Index]]
