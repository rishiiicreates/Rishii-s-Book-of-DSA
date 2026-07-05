# Minimum Knight Steps to Target Position

## Problem Statement
- given an $N \times N$ chessboard, a Knight's starting coordinate $(S_x, S_y)$, and a destination coordinate $(T_x, T_y)$, mathematically compute the minimum number of discrete legal moves required for the Knight to travel from the start to the target.

- *movement Rules:* A Knight moves in an $L$-shape. From $(x, y)$, it can mathematically transition to exactly eight possible coordinates $(x \pm 2, y \pm 1)$ and $(x \pm 1, y \pm 2)$, assuming the resulting coordinates reside strictly within the bounds $[0, N-1]$.


## Approach: Breadth-First Search (BFS)

- finding the absolute shortest path on an unweighted, discrete topological space is the canonical application of [[Breadth-First Search]] (BFS). The chessboard maps conceptually to an unweighted graph where every valid board coordinate is a node, and every legal Knight move establishes a directed edge.

- **topological Representation:** Maintain a queue of structures storing `(current_x, current_y, steps_taken)`.
- **state Tracking:** Initialize a boolean grid `visited[N][N]` to track previously visited coordinates, preventing infinite cycles and redundant calculations.
- **queue Initialization:** Enqueue the starting coordinate $(S_x, S_y, 0)$ and mark it as `visited`.
- **traversal:** While the queue is not empty:
   - dequeue the front state $(x, y, \text{dist})$.
   - if $(x, y) == (T_x, T_y)$, return $\text{dist}$.
   - iterate through the 8 predefined directional transformations vectors.
   - for each transformation resulting in a valid $(nx, ny)$ that has **not** been `visited`, mark it `visited` and enqueue $(nx, ny, \text{dist} + 1)$.

> [!important]
> For unbounded boards (e.g., an infinite plane), bounding algorithms (such as bidirectional BFS or $A^*$ utilizing Manhattan distance heuristics) become strictly mathematically necessary to prevent exponential explosion.


## Code Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

// Directional vectors mapping the 8 valid Knight transformations
int dx[] = {-2, -1, 1, 2, -2, -1, 1, 2};
int dy[] = {-1, -2, -2, -1, 1, 2, 2, 1};

struct Cell {
    int x, y, dist;
};

bool isValid(int x, int y, int N) {
    return (x >= 0 && x < N && y >= 0 && y < N);
}

int minStepToReachTarget(vector<int>& KnightPos, vector<int>& TargetPos, int N) {
    // Convert 1-based coordinates to 0-based indexing if necessary
    int startX = KnightPos[0] - 1;
    int startY = KnightPos[1] - 1;
    int targetX = TargetPos[0] - 1;
    int targetY = TargetPos[1] - 1;
    
    if (startX == targetX && startY == targetY) return 0;
    
    queue<Cell> q;
    vector<vector<bool>> visited(N, vector<bool>(N, false));
    
    q.push({startX, startY, 0});
    visited[startX][startY] = true;
    
    while (!q.empty()) {
        Cell current = q.front();
        q.pop();
        
        if (current.x == targetX && current.y == targetY) {
            return current.dist;
        }
        
        // Explore the 8 topological edges
        for (int i = 0; i < 8; ++i) {
            int nx = current.x + dx[i];
            int ny = current.y + dy[i];
            
            if (isValid(nx, ny, N) && !visited[nx][ny]) {
                visited[nx][ny] = true;
                q.push({nx, ny, current.dist + 1});
            }
        }
    }
    
    return -1; // Unreachable
}
```


## Complexity Analysis
- **time Complexity:** $O(N^2)$. The board contains $N^2$ unique states (nodes). In the worst-case exploration pattern, the BFS verifies all $N^2$ nodes exactly once, analyzing at most 8 constant edges per node.
- **space Complexity:** $O(N^2)$ utilized by the `visited` boolean matrix and the worst-case footprint of the auxiliary exploration `queue`.

NEXT: [[Index]]
