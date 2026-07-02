---
type: concept
tags: [prefix sum, cpp, math]
date: 2026-06-30
---
# Count with Sum Divisible By K

## Problem Statement
Count the number of continuous subarrays whose sum is divisible by `K`.

## Approach / Intuition
Compute the [[Prefix Sum]] modulo `K` and track the frequency of each remainder using a [[Hash Map]] or frequency array. If a remainder has been seen before, it means there are subarrays ending at the current index that are divisible by `K`. Add the previously seen frequency to the total count. Handle negative remainders properly.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the size of the array.
- **[[Space Complexity]]:** O(K) for the remainder frequency array or map.

## Sample Code
```cpp
int subarraysDivByK(vector<int>& nums, int k) {
    vector<int> remCount(k, 0);
    remCount[0] = 1;
    int count = 0, prefixSum = 0;
    for (int num : nums) {
        prefixSum += num;
        int rem = prefixSum % k;
        if (rem < 0) rem += k;
        
        count += remCount[rem];
        remCount[rem]++;
    }
    return count;
}
```

## New Keywords / STL Used
- `std::vector` for frequency tracking

## Edge Cases
- All elements are divisible by `K`
- Negative numbers resulting in negative prefix sums
- `K` is 1 (all subarrays match)
