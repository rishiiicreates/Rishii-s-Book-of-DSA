# Rat in a Maze

## Problem Statement
- given a binary [[Matrix]] representing a maze where 1 is an open path and 0 is a blocked cell, find all possible paths for a rat to move from the top-left to the bottom-right corner.

## Approach / Intuition
- employ [[Backtracking]] alongside [[Depth-First Search]] on the [[Matrix]]. From the current cell, recursively explore all four valid orthogonal directions. Mark the current cell as visited to prevent cycles, and backtrack by unmarking it before returning from the recursive call. Accumulate valid paths when the destination cell is reached.

## Time & Space Complexity
- **[[time Complexity]]:** $O(4^{(N^2)})$
- **[[space Complexity]]:** $O(N^2)$ for recursion stack

## Sample Code
```cpp
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

void solve(int i, int j, vector<vector<int>>& m, int n, string path, vector<string>& ans, vector<vector<int>>& visited) {
    if (i == n - 1 && j == n - 1) {
        ans.push_back(path);
        return;
    }
    
    int di[] = {1, 0, 0, -1};
    int dj[] = {0, -1, 1, 0};
    char move[] = {'D', 'L', 'R', 'U'};
    
    for (int k = 0; k < 4; k++) {
        int next_i = i + di[k];
        int next_j = j + dj[k];
        if (next_i >= 0 && next_j >= 0 && next_i < n && next_j < n && !visited[next_i][next_j] && m[next_i][next_j] == 1) {
            visited[i][j] = 1;
            solve(next_i, next_j, m, n, path + move[k], ans, visited);
            visited[i][j] = 0;
        }
    }
}

vector<string> findPath(vector<vector<int>>& m, int n) {
    vector<string> ans;
    if (m[0][0] == 0) return ans;
    vector<vector<int>> visited(n, vector<int>(n, 0));
    solve(0, 0, m, n, "", ans, visited);
    sort(ans.begin(), ans.end());
    return ans;
}
```

## New Keywords / STL Used
- direction arrays

## Edge Cases
- start or end cell is blocked, no valid path exists, $1 \times 1$ maze.

NEXT: [[Index]]
