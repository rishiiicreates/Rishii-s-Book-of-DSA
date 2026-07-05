# Count Subarrays with K Odd Numbers

## Problem Statement
- given an array of integers, count the number of continuous subarrays that contain exactly `k` odd numbers.

## Approach / Intuition
- this problem can be transformed by replacing odd numbers with `1` and even numbers with `0`. The problem then becomes finding the number of subarrays with a sum exactly equal to `k`. We can solve this efficiently by applying a [[Prefix Sum]] map or by calculating `atMost(k) - atMost(k - 1)` using a standard [[Sliding Window]].

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
int atMost(const vector<int>& nums, int k) {
    if (k < 0) return 0;
    int count = 0, res = 0, left = 0;
    for (int right = 0; right < nums.size(); ++right) {
        if (nums[right] % 2 != 0) count++;
        while (count > k) {
            if (nums[left] % 2 != 0) count--;
            left++;
        }
        res += (right - left + 1);
    }
    return res;
}

int numberOfSubarrays(const vector<int>& nums, int k) {
    return atMost(nums, k) - atMost(nums, k - 1);
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- empty array, `k = 0`, no odd numbers in array, fewer than `k` odd numbers total.

NEXT: [[Index]]
