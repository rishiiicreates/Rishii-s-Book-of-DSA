# Stable Marriage

## Problem Statement
- given N men and N women, where each person has ranked all members of the opposite sex in order of preference, marry the men and women together such that there are no two people of opposite sex who would both rather have each other than their current partners.

## Approach / Intuition
- use the Gale-Shapley algorithm, which is a classic [[Greedy Algorithm]]. Unengaged men propose to the most preferred woman they haven't proposed to yet. The woman accepts if she is free or prefers him over her current partner. This continues until all men are engaged.

## Time & Space Complexity
- **[[time Complexity]]:** O(N^2)
- **[[space Complexity]]:** O(N^2) for preference matrices

## Sample Code
```cpp
#include <vector>
#include <iostream>

using namespace std;

bool prefers(const vector<vector<int>>& prefer, int w, int m, int m1, int N) {
    for (int i = 0; i < N; i++) {
        if (prefer[w][i] == m) return true;
        if (prefer[w][i] == m1) return false;
    }
    return false;
}

void stableMarriage(const vector<vector<int>>& prefer, int N) {
    vector<int> wPartner(N, -1);
    vector<bool> mFree(N, true);
    int freeCount = N;
    while (freeCount > 0) {
        int m;
        for (m = 0; m < N; m++)
            if (mFree[m]) break;
        for (int i = 0; i < N && mFree[m]; i++) {
            int w = prefer[m][i];
            if (wPartner[w - N] == -1) {
                wPartner[w - N] = m;
                mFree[m] = false;
                freeCount--;
            } else {
                int m1 = wPartner[w - N];
                if (prefers(prefer, w, m, m1, N)) {
                    wPartner[w - N] = m;
                    mFree[m] = false;
                    mFree[m1] = true;
                }
            }
        }
    }
}
```

## New Keywords / STL Used
- 2d `vector`

## Edge Cases
- universal agreement on preferences
- perfect opposite preferences

NEXT: [[Index]]
