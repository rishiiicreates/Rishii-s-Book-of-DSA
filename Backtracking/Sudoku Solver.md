# Sudoku Solver

## Problem Statement
- solve a given $9 \times 9$ Sudoku board by filling empty cells such that every row, column, and $3 \times 3$ sub-grid contains the digits $1$ to $9$ without repetition.

## Approach / Intuition
- traverse the [[Matrix]] to find an empty cell. For this cell, attempt placing digits from $1$ to $9$. Validate each placement against the row, column, and sub-grid constraints. If a placement is valid, proceed recursively using [[Backtracking]]. If the recursive call fails, undo the placement and try the next digit.

## Time & Space Complexity
- **[[time Complexity]]:** $O(9^{(N^2)})$ where $N$ is empty cells count
- **[[space Complexity]]:** $O(N^2)$ for recursive call depth

## Sample Code
```cpp
#include <vector>

using namespace std;

bool isValid(vector<vector<char>>& board, int row, int col, char c) {
    for (int i = 0; i < 9; i++) {
        if (board[row][i] == c) return false;
        if (board[i][col] == c) return false;
        if (board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c) return false;
    }
    return true;
}

bool solve(vector<vector<char>>& board) {
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] == '.') {
                for (char c = '1'; c <= '9'; c++) {
                    if (isValid(board, i, j, c)) {
                        board[i][j] = c;
                        if (solve(board)) return true;
                        board[i][j] = '.';
                    }
                }
                return false;
            }
        }
    }
    return true;
}

void solveSudoku(vector<vector<char>>& board) {
    solve(board);
}
```

## New Keywords / STL Used
- sub-grid indexing

## Edge Cases
- board already filled, board with no valid solution, board with multiple solutions (finds first).

NEXT: [[Index]]
