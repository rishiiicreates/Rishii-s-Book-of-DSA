---
type: concept
tags: [array, cpp, traversal]
date: 2026-06-30
---
# Leaders in an Array

## Problem Statement
Given an array $A$ of size $N$, find all the leaders in the array. An element is defined as a leader if it is strictly greater than all elements to its right. The rightmost element is always considered a leader.

*Example:* $A = [16, 17, 4, 3, 5, 2]$
*Result:* $[17, 5, 2]$

---

## Approach: Reverse Array Traversal (Optimal)

A naive approach would check all elements to the right for every element, taking $O(N^2)$ time.
Instead, we can iterate from the right to the left (a reverse [[Array Traversal]]).

1. Maintain a `maxRight` variable tracking the maximum element seen so far from the right.
2. Since the rightmost element is always a leader, we initialize `maxRight` with `A[N-1]` and add it to our list.
3. For every element from $N-2$ down to $0$, if it is strictly greater than `maxRight`, it is a leader. We update `maxRight` and add it to our list.
4. Because we traversed backwards, the leaders are collected from right to left. We must reverse the list to maintain the original left-to-right order.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

vector<int> findLeaders(const vector<int>& arr) {
    int n = arr.size();
    vector<int> leaders;
    if (n == 0) return leaders;
    
    // The rightmost element is always a leader
    int maxRight = arr[n - 1];
    leaders.push_back(maxRight);
    
    // Traverse from right to left
    for (int i = n - 2; i >= 0; --i) {
        if (arr[i] > maxRight) {
            maxRight = arr[i];
            leaders.push_back(maxRight);
        }
    }
    
    // Reverse to maintain the original array's relative order
    reverse(leaders.begin(), leaders.end());
    
    return leaders;
}
```

> [!tip]
> `std::reverse(begin, end)` is an efficient $O(K)$ operation where $K$ is the number of leaders found.

> [!warning]
> The problem statement says "strictly greater". Thus, if `arr[i] == maxRight`, it is **not** a leader, and `maxRight` does not change.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We make a single pass through the array. Reversing the output takes $O(K)$ where $K \le N$, making the overall time bound strictly $O(N)$.
- **Space Complexity:** $O(K)$ auxiliary space for the output vector, where $K$ is the number of leaders. In the worst case (a descending array), $K = N$, so $O(N)$ space.
