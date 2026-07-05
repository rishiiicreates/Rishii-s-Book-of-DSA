# Subarrays with All Distinct Elements

## Problem Statement
- given an array, calculate the total number of contiguous subarrays consisting of all distinct elements.

## Approach / Intuition
- use the [[Sliding Window]] method with an unordered set or boolean frequency [[Array]] to keep track of elements in the current window. Expand the right boundary, adding elements. If a duplicate is encountered, shrink from the left boundary until the duplicate is removed. At each step, all subarrays ending at the right pointer and starting anywhere from `left` to `right` are valid, contributing `right - left + 1` to the total count.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>
#include <unordered_set>

using namespace std;

int sumOfLength(vector<int>& arr) {
    unordered_set<int> window;
    int left = 0, right = 0;
    int ans = 0;
    
    while (right < arr.size()) {
        while (window.count(arr[right])) {
            window.erase(arr[left]);
            left++;
        }
        window.insert(arr[right]);
        ans += (right - left + 1);
        right++;
    }
    return ans;
}
```

## New Keywords / STL Used
- `unordered_set`, `insert`, `count`, `erase`

## Edge Cases
- array with all identical elements, array with all unique elements, single element array.

NEXT: [[Index]]
