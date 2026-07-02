---
type: concept
tags: [graph, dfs, bfs, matrix, cpp, boundaries]
date: 2026-07-01
---
# Surrounded Regions (Replace O's with X's)

## Problem Statement
Given an $M \times N$ spatial matrix grid, structurally mapping characters `'X'` and `'O'`. Mathematically mutate every generic `'O'` that is fully isolated geographically by `'X'` on all 4 directions into `'X'`.
The strict algebraic law states that any `'O'` structurally bound to the perimeter (the 4 spatial boundaries of the matrix grid) is mathematically invincible to geometric capture. Subsequently, any interior `'O'` contiguous with an invincible perimeter `'O'` is also immune via transitive property inheritance.

---

## Approach: Reverse Propagation Analysis (DFS/BFS)

Structurally iterating the center coordinates and attempting to prove total isolation evaluates to a convoluted logic geometry.
Instead, we algebraically reverse the topological constraint constraint:
1. Isolate the mathematical perimeter completely (Row $0$, Row $M-1$, Col $0$, Col $N-1$).
2. Identify all `'O'` scalars geometrically bound to this absolute boundary.
3. Utilize DFS (or BFS) localized on these boundary `'O'` nodes to topologically map all internal contiguous `'O'` neighbors, dynamically mutating them to an invincible placeholder state (e.g., `'T'`).
4. Execute a complete sequential sweep of the $M \times N$ matrix.
   - Any surviving `'O'` was mathematically isolated and is mutated to `'X'`.
   - Any placeholder `'T'` is returned to its invincible state `'O'`.

---

## Code Implementation

```cpp
#include <vector>

using namespace std;

void dfs(int r, int c, vector<vector<char>>& board, int dr[], int dc[]) {
    // Topologically mark as invincible boundary geometry
    board[r][c] = 'T';
    
    int m = board.size();
    int n = board[0].size();
    
    // Execute structural spatial expansion
    for (int i = 0; i < 4; i++) {
        int nr = r + dr[i];
        int nc = c + dc[i];
        
        // Evaluate neighbor scalar constraint compatibility
        if (nr >= 0 && nr < m && nc >= 0 && nc < n && board[nr][nc] == 'O') {
            dfs(nr, nc, board, dr, dc);
        }
    }
}

void solve(vector<vector<char>>& board) {
    if (board.empty()) return;
    
    int m = board.size();
    int n = board[0].size();
    
    int dr[] = {-1, 0, 1, 0};
    int dc[] = {0, 1, 0, -1};
    
    // Topologically evaluate Left/Right boundary boundaries
    for (int i = 0; i < m; i++) {
        if (board[i][0] == 'O') dfs(i, 0, board, dr, dc);
        if (board[i][n-1] == 'O') dfs(i, n-1, board, dr, dc);
    }
    
    // Topologically evaluate Top/Bottom boundary boundaries
    for (int j = 0; j < n; j++) {
        if (board[0][j] == 'O') dfs(0, j, board, dr, dc);
        if (board[m-1][j] == 'O') dfs(m-1, j, board, dr, dc);
    }
    
    // Final Geometry Sweep and Mutation
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 'O') {
                board[i][j] = 'X'; // Geometrically captured
            } else if (board[i][j] == 'T') {
                board[i][j] = 'O'; // Invincible topological perimeter
            }
        }
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(M \times N)$ total boundary constraint. DFS isolates topological components strictly linearly. The initial boundary sweep and the terminal replacement operate at $O(M \times N)$.
- **Space Complexity:** $O(M \times N)$ bound specifically to the DFS recursive call-stack geometric limits. In strict production matrices, BFS operates with identical mathematical bounds but negates call-stack overflow vulnerabilities.

> [!important]
> **State Machine Memory Avoidance:** Utilizing a topological `visited` matrix maps generic graphs structurally, but for strict 2-state mutations (`'X'` and `'O'`), utilizing a third scalar state (`'T'`) directly within the array eliminates auxiliary space allocations dynamically.
