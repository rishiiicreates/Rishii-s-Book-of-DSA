---
type: concept
tags: [backtracking, cpp, matrix]
date: 2026-06-30
---
# Knight's Tour

## Problem Statement
Find a path for a knight on an $N \times N$ chessboard such that the knight visits every cell exactly once.

## Approach / Intuition
Apply [[Backtracking]] across the [[Matrix]]. The knight has 8 possible moves from any cell. At each step, iterate through these moves, check if the new cell is valid and unvisited, mark it with the current move number, and recurse. If the sequence reaches $N^2$, the tour is complete. If a move leads to a dead end, backtrack by unmarking the cell.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(8^{(N^2)})$
- **[[Space Complexity]]:** $O(N^2)$ for recursion stack and board

## Sample Code
```cpp
#include <vector>
#include <iostream>

using namespace std;

int dx[] = { 2, 1, -1, -2, -2, -1, 1, 2 };
int dy[] = { 1, 2, 2, 1, -1, -2, -2, -1 };

bool solveKTUtil(int x, int y, int movei, vector<vector<int>>& sol, int N) {
    if (movei == N * N) return true;
    
    for (int k = 0; k < 8; k++) {
        int next_x = x + dx[k];
        int next_y = y + dy[k];
        
        if (next_x >= 0 && next_x < N && next_y >= 0 && next_y < N && sol[next_x][next_y] == -1) {
            sol[next_x][next_y] = movei;
            if (solveKTUtil(next_x, next_y, movei + 1, sol, N)) return true;
            sol[next_x][next_y] = -1;
        }
    }
    return false;
}

void solveKT(int N) {
    vector<vector<int>> sol(N, vector<int>(N, -1));
    sol[0][0] = 0;
    solveKTUtil(0, 0, 1, sol, N);
}
```

## New Keywords / STL Used
Knights move arrays

## Edge Cases
Small values of $N$ where no tour exists ($N=1$ works, $N=2, 3, 4$ fail), non-square boards.
