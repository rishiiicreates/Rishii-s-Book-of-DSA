# Minimize the Max Height Diff

## Problem Statement
- given an array of heights of N towers and a positive integer K, you have to increase or decrease the height of every tower by exactly K. Find the minimum difference between the maximum and minimum heights of the modified towers.

## Approach / Intuition
- sort the array. The maximum difference initially is `arr[n-1] - arr[0]`. Iterate through the array and try to make `arr[i-1] + k` the maximum and `arr[i] - k` the minimum (if `arr[i] >= k`). Use a [[Greedy Algorithm]] to track the minimal possible difference across all splits.

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int getMinDiff(vector<int>& arr, int n, int k) {
    sort(arr.begin(), arr.end());
    int ans = arr[n - 1] - arr[0];
    int smallest = arr[0] + k;
    int largest = arr[n - 1] - k;
    for (int i = 0; i < n - 1; i++) {
        int mini = min(smallest, arr[i + 1] - k);
        int maxi = max(largest, arr[i] + k);
        if (mini < 0) continue;
        ans = min(ans, maxi - mini);
    }
    return ans;
}
```

## New Keywords / STL Used
- `min`, `max`

## Edge Cases
- all elements same initially
- subtraction results in a negative height (ignored)

NEXT: [[Index]]
