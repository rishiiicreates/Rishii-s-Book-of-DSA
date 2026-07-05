# Word Search

## Problem Statement
- given an $M \times N$ grid of characters and a string word, return true if the word exists in the grid. The word can be constructed from letters of sequentially adjacent cells.

## Approach / Intuition
- iterate through the [[[[Matrix]]]] to find the starting character of the [[String]]. Upon finding a match, initiate a [[Backtracking]] search in all four directions. Temporarily mutate the cell value (e.g., to a special character) to mark it as visited, avoiding a separate visited array. Restore the character when backtracking.

## Time & Space Complexity
- **[[time Complexity]]:** $O(M \cdot N \cdot 3^L)$ where $L$ is word length
- **[[space Complexity]]:** $O(L)$ for recursion stack

## Sample Code
```cpp
#include <vector>
#include <string>

using namespace std;

bool backtrack(vector<vector<char>>& board, string& word, int i, int j, int index) {
    if (index == word.length()) return true;
    if (i < 0 || i >= board.size() || j < 0 || j >= board[0].size() || board[i][j] != word[index]) return false;
    
    char temp = board[i][j];
    board[i][j] = '#';
    
    bool found = backtrack(board, word, i + 1, j, index + 1) ||
                 backtrack(board, word, i - 1, j, index + 1) ||
                 backtrack(board, word, i, j + 1, index + 1) ||
                 backtrack(board, word, i, j - 1, index + 1);
                 
    board[i][j] = temp;
    return found;
}

bool exist(vector<vector<char>>& board, string word) {
    for (int i = 0; i < board.size(); i++) {
        for (int j = 0; j < board[0].size(); j++) {
            if (backtrack(board, word, i, j, 0)) return true;
        }
    }
    return false;
}
```

## New Keywords / STL Used
- in-place modification

## Edge Cases
- word longer than total cells, single character word, board has only one cell.

NEXT: [[Index]]
