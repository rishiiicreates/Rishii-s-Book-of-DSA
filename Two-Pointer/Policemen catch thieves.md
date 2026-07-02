---
type: concept
tags: [two-pointer, cpp, greedy, array]
date: 2026-06-30
---
# Policemen Catch Thieves

## Problem Statement
Given an array containing 'P' (Policeman) and 'T' (Thief) and a maximum distance $K$, find the maximum number of thieves that can be caught. A policeman can catch at most one thief who is no more than $K$ units away.

## Approach / Intuition
Store the indices of all policemen and thieves in separate lists. Use a [[Two-Pointer]] approach on both lists. Compare the current policeman index with the current thief index. If their absolute distance is within $K$, increment the catch count and move both pointers. Otherwise, advance the pointer corresponding to the smaller index, as it is the most restricting factor for future matches.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>
#include <cmath>
#include <string>

using namespace std;

int catchThieves(vector<char>& arr, int n, int k) {
    vector<int> pol, thi;
    for (int i = 0; i < n; i++) {
        if (arr[i] == 'P') pol.push_back(i);
        else if (arr[i] == 'T') thi.push_back(i);
    }
    
    int l = 0, r = 0, res = 0;
    while (l < pol.size() && r < thi.size()) {
        if (abs(pol[l] - thi[r]) <= k) {
            res++;
            l++;
            r++;
        } else if (pol[l] < thi[r]) {
            l++;
        } else {
            r++;
        }
    }
    return res;
}
```

## New Keywords / STL Used
`abs`

## Edge Cases
No policemen, no thieves, $K=0$.
