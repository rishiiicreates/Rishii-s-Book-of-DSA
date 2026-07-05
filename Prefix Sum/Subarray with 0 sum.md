# Subarray with 0 sum

## Problem Statement
- determine if there exists a contiguous subarray with a sum equal to 0.

## Approach / Intuition
- compute the [[Prefix Sum]] as you iterate through the array. If the prefix sum becomes 0, or if a prefix sum value is repeated (which we can check using a [[Hash Set]]), it means the elements between the two identical prefix sums add up to 0.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the length of the array.
- **[[space Complexity]]:** O(N) to store prefix sums.

## Sample Code
```cpp
bool hasZeroSumSubarray(vector<int>& arr) {
    unordered_set<int> seenSums;
    int prefixSum = 0;
    for (int num : arr) {
        prefixSum += num;
        if (prefixSum == 0 || seenSums.find(prefixSum) != seenSums.end()) {
            return true;
        }
        seenSums.insert(prefixSum);
    }
    return false;
}
```

## New Keywords / STL Used
- `std::unordered_set`

## Edge Cases
- array contains a single 0
- entire array sums to 0
- no such subarray exists

NEXT: [[Index]]
