# Range Queries

## Problem Statement
- answer multiple queries asking for the sum of elements within a specific range $[L, R]$ of an array efficiently.

## Approach / Intuition
- without precomputation, each query takes $O(N)$ time. By precomputing a [[Prefix Sum Array]], we can answer each query in $O(1)$ time. The sum of the range $[L, R]$ is calculated as `prefix[R] - prefix[L - 1]`. If $L$ is $0$, the sum is just `prefix[R]`.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ for precomputation, $O(1)$ per query
- **[[space Complexity]]:** $O(N)$ for the prefix sum array

## Sample Code
```cpp
#include <vector>

class RangeQuery {
private:
    std::vector<int> prefix;
public:
    RangeQuery(std::vector<int>& nums) {
        if (nums.empty()) return;
        prefix.resize(nums.size());
        prefix[0] = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            prefix[i] = prefix[i - 1] + nums[i];
        }
    }
    
    int query(int left, int right) {
        if (left == 0) return prefix[right];
        return prefix[right] - prefix[left - 1];
    }
};
```

## New Keywords / STL Used
- `std::vector::resize`

## Edge Cases
- $l = 0$
- $l = R$ (single element)
- invalid range where $L > R$ or out of bounds (can add boundary checks)

NEXT: [[Index]]
