# Count Subarrays with K Equal Value Pairs

## Problem Statement
- given an array, find the number of contiguous subarrays that contain exactly `k` identical value pairs. (Note: A pair (i, j) is identical if `arr[i] == arr[j]`).

## Approach / Intuition
- use a [[Sliding Window]] with a frequency map to track identical pairs. When expanding the window, adding an element with frequency `f` adds `f` new pairs. Calculate `atMost(k)` subarrays and return `atMost(k) - atMost(k-1)`. Shrink the window when the pair count exceeds the threshold.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(n) for the hash map

## Sample Code
```cpp
long long atMostPairs(const vector<int>& nums, int k) {
    if (k < 0) return 0;
    unordered_map<int, int> freq;
    long long pairs = 0, count = 0;
    int left = 0;
    for (int right = 0; right < nums.size(); ++right) {
        pairs += freq[nums[right]];
        freq[nums[right]]++;
        while (pairs > k) {
            freq[nums[left]]--;
            pairs -= freq[nums[left]];
            left++;
        }
        count += (right - left + 1);
    }
    return count;
}

long long subarraysWithKPairs(const vector<int>& nums, int k) {
    return atMostPairs(nums, k) - atMostPairs(nums, k - 1);
}
```

## New Keywords / STL Used
- `unordered_map`, `long long`

## Edge Cases
- `k = 0`, no duplicate elements, empty array, extremely large frequency counts.

NEXT: [[Index]]
