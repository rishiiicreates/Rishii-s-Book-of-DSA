# Fruit Into Baskets

## Problem Statement
- find the maximum length of a contiguous subarray that contains at most 2 distinct elements (fruits).

## Approach / Intuition
- use a [[Sliding Window]] paired with a [[Hash Map]] to track the frequency of each element in the current window. Expand the right boundary, and if the map size exceeds 2, shrink the left boundary until the size becomes 2 again.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the size of the array.
- **[[space Complexity]]:** O(1) since the map stores at most 3 distinct elements.

## Sample Code
```cpp
int totalFruit(vector<int>& fruits) {
    unordered_map<int, int> count;
    int left = 0, maxLen = 0;
    for (int right = 0; right < fruits.size(); ++right) {
        count[fruits[right]]++;
        while (count.size() > 2) {
            count[fruits[left]]--;
            if (count[fruits[left]] == 0) {
                count.erase(fruits[left]);
            }
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## New Keywords / STL Used
- `std::unordered_map`

## Edge Cases
- array contains only 1 or 2 distinct elements overall
- empty array
- array with all unique elements

NEXT: [[Index]]
