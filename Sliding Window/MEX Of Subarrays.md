---
type: concept
tags: [sliding window, cpp, mex]
date: 2026-06-30
---
# MEX Of Subarrays

## Problem Statement
Given an array and an integer `K`, find the Minimum Excluded (MEX) value for every contiguous subarray of size `K`. The MEX is the smallest non-negative integer strictly not present in the set.

## Approach / Intuition
Initialize a set containing all numbers from `0` to `K` (the potential missing numbers) and use a frequency map. As the window traverses the array, update frequencies. When a frequency becomes zero, insert the element back into the missing set; when an element's frequency reaches one, erase it from the missing set. The MEX is permanently the smallest element retrieved via `*missing.begin()`, cleanly fusing [[Sliding Window]] with a [[Balanced BST]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log K)
- **[[Space Complexity]]:** O(K)

## Sample Code
```cpp
vector<int> mexOfSubarrays(vector<int>& arr, int k) {
    vector<int> res;
    if (k > arr.size()) return res;
    unordered_map<int, int> freq;
    set<int> missing;
    for (int i = 0; i <= k; i++) missing.insert(i);
    for (int i = 0; i < k; i++) {
        freq[arr[i]]++;
        missing.erase(arr[i]);
    }
    res.push_back(*missing.begin());
    for (int i = k; i < arr.size(); i++) {
        freq[arr[i]]++;
        missing.erase(arr[i]);
        freq[arr[i - k]]--;
        if (freq[arr[i - k]] == 0) {
            missing.insert(arr[i - k]);
        }
        res.push_back(*missing.begin());
    }
    return res;
}
```

## New Keywords / STL Used
`std::set`, `*missing.begin()`

## Edge Cases
All array elements are extremely large (MEX is perpetually 0), tightly bound arrays containing `0` to `K-1`.
