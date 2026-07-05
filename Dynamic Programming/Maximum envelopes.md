# Russian Doll Envelopes

## Problem Statement
- given a 2D array of envelopes with width and height, find the maximum number of envelopes you can Russian doll (put one inside another).

## Approach / Intuition
- sort the envelopes by width in ascending order. If widths are equal, sort by height in descending order. This clever sorting reduces the problem to finding the [[Longest Increasing Subsequence]] based purely on heights. Since equal widths have descending heights, they cannot form an increasing subsequence, which correctly prevents putting an envelope inside another of the same width.

## Time & Space Complexity
- **[[time Complexity]]:** O(N \log N)
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int maxEnvelopes(vector<vector<int>>& envelopes) {
    sort(envelopes.begin(), envelopes.end(), [](const vector<int>& a, const vector<int>& b) {
        if (a[0] == b[0]) {
            return a[1] > b[1];
        }
        return a[0] < b[0];
    });
    
    vector<int> tails;
    for (const auto& env : envelopes) {
        int height = env[1];
        auto it = lower_bound(tails.begin(), tails.end(), height);
        if (it == tails.end()) {
            tails.push_back(height);
        } else {
            *it = height;
        }
    }
    
    return tails.size();
}
```

## New Keywords / STL Used
- `std::sort` with custom lambda, `std::lower_bound`

## Edge Cases
- empty array, envelopes with identical dimensions, strictly decreasing sizes.

NEXT: [[Index]]
