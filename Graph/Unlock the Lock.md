---
type: concept
tags: [graph, cpp, bfs, shortest-path]
date: 2026-06-30
---
# Unlock the Lock

## Problem Statement
You have a 4-wheel lock. You want to reach a target combination starting from "0000", but cannot pass through deadends. Find the minimum turns required.

## Approach / Intuition
Use [[BFS]] to find the shortest path. At each step, generate 8 possible next combinations by turning each of the 4 wheels up or down. Skip if it's a deadend or already visited.

## Time & Space Complexity
- **[[Time Complexity]]:** O(10^4)
- **[[Space Complexity]]:** O(10^4)

## Sample Code
```cpp
int openLock(vector<string>& deadends, string target) {
    unordered_set<string> dead(deadends.begin(), deadends.end());
    if (dead.count("0000")) return -1;
    if (target == "0000") return 0;
    queue<pair<string, int>> q;
    unordered_set<string> vis;
    q.push({"0000", 0});
    vis.insert("0000");
    while (!q.empty()) {
        string s = q.front().first;
        int moves = q.front().second;
        q.pop();
        if (s == target) return moves;
        for (int i = 0; i < 4; i++) {
            for (int d = -1; d <= 1; d += 2) {
                string next = s;
                next[i] = (next[i] - '0' + d + 10) % 10 + '0';
                if (!dead.count(next) && !vis.count(next)) {
                    vis.insert(next);
                    q.push({next, moves + 1});
                }
            }
        }
    }
    return -1;
}
```

## New Keywords / STL Used
`unordered_set`, string manipulation

## Edge Cases
Target is "0000", "0000" is a deadend, impossible to reach target.
