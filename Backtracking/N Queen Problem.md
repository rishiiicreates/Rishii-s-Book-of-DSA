# N Queen Problem

## Problem Statement
- place $N$ queens on an $N \times N$ chessboard such that no two queens threaten each other.

## Approach / Intuition
- use [[Backtracking]] to place one queen per row. For each column in the current row, verify if placing a queen is safe by checking the same column and both diagonals. To optimize safety checks, maintain [[Array]]s acting as hash maps to track occupied columns and both diagonals, rather than scanning the board.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N!)$
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>
#include <string>

using namespace std;

void solve(int row, int n, vector<string>& board, vector<vector<string>>& ans, vector<int>& col, vector<int>& diag1, vector<int>& diag2) {
    if (row == n) {
        ans.push_back(board);
        return;
    }
    
    for (int c = 0; c < n; c++) {
        if (col[c] || diag1[row + c] || diag2[row - c + n - 1]) continue;
        
        board[row][c] = 'Q';
        col[c] = diag1[row + c] = diag2[row - c + n - 1] = 1;
        
        solve(row + 1, n, board, ans, col, diag1, diag2);
        
        board[row][c] = '.';
        col[c] = diag1[row + c] = diag2[row - c + n - 1] = 0;
    }
}

vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> ans;
    vector<string> board(n, string(n, '.'));
    vector<int> col(n, 0), diag1(2 * n, 0), diag2(2 * n, 0);
    solve(0, n, board, ans, col, diag1, diag2);
    return ans;
}
```

## New Keywords / STL Used
- hashing diagonals

## Edge Cases
- $n=1$ (trivial), $N=2, 3$ (no solution).

NEXT: [[Index]]
