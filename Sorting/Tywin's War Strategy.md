---
type: concept
tags: [sorting, cpp, greedy-matching]
date: 2026-06-30
---
# Tywin's War Strategy

## Problem Statement
Tywin Lannister wants to win a war against an enemy army. Given an array $T$ representing the strength of Tywin's battalions and an array $E$ representing the strength of the enemy's battalions, find the maximum number of enemy battalions Tywin can defeat. 

A battalion from $T$ defeats a battalion from $E$ if and only if its strength is strictly greater than the enemy's. Each battalion can fight at most once.

*Example:* $T = [10, 20, 30]$, $E = [15, 25, 35]$
*Result:* $2$ ($20$ beats $15$, $30$ beats $25$)

---

## Approach: Greedy Matching with Two Pointers

This is a classic bipartite matching problem on a sorted array, solved optimally via a [[Greedy Algorithm]] using [[Two Pointers]].

1. Sort both arrays $T$ and $E$ in strictly ascending order.
2. Use two pointers: `i` for Tywin's army and `j` for the Enemy's army.
3. Compare $T[i]$ and $E[j]$.
4. If $T[i] > E[j]$, Tywin's battalion wins! This is the most efficient victory because $E[j]$ is the weakest undefeated enemy, and $T[i]$ is the weakest available force that can beat it. Increment both `i` and `j` and increase the win count.
5. If $T[i] \le E[j]$, Tywin's current battalion is too weak to defeat the enemy's current weakest battalion. We just move `i` forward to find a stronger battalion.
6. The process ends when either army runs out of available battalions.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int maxWins(vector<int>& tywin, vector<int>& enemy) {
    sort(tywin.begin(), tywin.end());
    sort(enemy.begin(), enemy.end());
    
    int wins = 0;
    int i = 0; // Pointer for Tywin
    int j = 0; // Pointer for Enemy
    
    while (i < tywin.size() && j < enemy.size()) {
        if (tywin[i] > enemy[j]) {
            wins++;
            i++;
            j++;
        } else {
            // Tywin's current battalion is too weak, skip it
            i++;
        }
    }
    
    return wins;
}
```

> [!tip]
> This strategy is mathematically identical to the "Assign Cookies" problem. The greedy choice is always safe: we match the smallest sufficient resource to the smallest demand.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N + M \log M)$. We must sort both arrays. The two-pointer traversal takes linear time $O(N + M)$.
- **Space Complexity:** $O(1)$ auxiliary space, ignoring the recursion stack of `std::sort`.
