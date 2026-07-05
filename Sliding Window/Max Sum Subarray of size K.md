# Max Sum Subarray of Size K

## Problem Statement
- given an array of integers and a number `K`, find the maximum sum of any contiguous subarray of exactly size `K`.

## Approach / Intuition
- compute the sum of the very first `K` elements. Then, advance the window one element at a time across the array, adding the incoming element and subtracting the outgoing element, continuously tracking the maximum sum observed. This establishes the fundamental fixed-size [[Sliding Window]] pattern.

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
int maximumSumSubarray(vector<int>& arr, int k) {
    if (k > arr.size()) return 0;
    int currentSum = 0, maxSum = 0;
    for (int i = 0; i < k; i++) {
        currentSum += arr[i];
    }
    maxSum = currentSum;
    for (int i = k; i < arr.size(); i++) {
        currentSum += arr[i] - arr[i - k];
        maxSum = max(maxSum, currentSum);
    }
    return maxSum;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- `k` greater than array length, array consisting strictly of negative numbers (where maximum sum remains negative).

NEXT: [[Index]]
