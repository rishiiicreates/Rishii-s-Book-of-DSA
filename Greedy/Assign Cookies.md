# Assign Cookies

## Problem Statement
- given an array of children's greed factors and an array of cookie sizes, maximize the number of content children. A child is content if they receive a cookie of size greater than or equal to their greed factor.

## Approach / Intuition
- sort both the greed factors and cookie sizes. Use a [[Two Pointers]] approach starting from the smallest cookie and smallest greed factor. If a cookie satisfies the current child, move both pointers. Otherwise, try the next larger cookie for the same child using the [[Greedy Algorithm]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N + M log M)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int findContentChildren(vector<int>& g, vector<int>& s) {
    sort(g.begin(), g.end());
    sort(s.begin(), s.end());
    int child = 0, cookie = 0;
    while (child < g.size() && cookie < s.size()) {
        if (s[cookie] >= g[child]) {
            child++;
        }
        cookie++;
    }
    return child;
}
```

## New Keywords / STL Used
- `sort`

## Edge Cases
- empty cookies array
- all cookies smaller than the smallest greed factor

NEXT: [[Index]]
