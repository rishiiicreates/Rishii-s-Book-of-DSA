# Chocolate Distribution

## Problem Statement
- given an array of `N` integers where each value represents the number of chocolates in a packet, distribute the packets among `M` students such that each student gets exactly one packet and the difference between the maximum and minimum number of chocolates given is aggressively minimized.

## Approach / Intuition
- first, sort the array in ascending order. Since the array is sorted, any valid contiguous subarray of size `M` dictates a potential distribution, with the minimum value at the front and maximum at the back. Evaluate the difference between boundaries while passing a fixed-size [[Sliding Window]] over the [[Sorting]] processed array to pinpoint the absolute lowest differential.

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N)
- **[[space Complexity]]:** O(log N)

## Sample Code
```cpp
long long findMinDiff(vector<long long> arr, long long n, long long m) {
    if (m == 0 || n == 0) return 0;
    sort(arr.begin(), arr.end());
    if (n < m) return -1;
    long long min_diff = LLONG_MAX;
    for (int i = 0; i + m - 1 < n; i++) {
        long long diff = arr[i + m - 1] - arr[i];
        if (diff < min_diff) {
            min_diff = diff;
        }
    }
    return min_diff;
}
```

## New Keywords / STL Used
- `lLONG_MAX`

## Edge Cases
- `m > N` (unsolvable), `M == 1` (zero difference), empty arrays triggering boundary errors.

NEXT: [[Index]]
