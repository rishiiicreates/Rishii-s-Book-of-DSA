# Min Swaps to group all 1's

## Problem Statement
- given a binary array, find the minimum number of swaps required to group all 1’s present in the array together in any single contiguous block.

## Approach / Intuition
- count the absolute total number of 1s in the array, let's call it `TotalOnes`. Then, search for a subarray of exactly size `TotalOnes` that already contains the maximum amount of 1s using a standard [[Sliding Window]]. The minimum swaps required is simply `TotalOnes` minus this maximum cluster of 1s encountered.

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
int minSwaps(vector<int>& arr) {
    int totalOnes = 0;
    for (int val : arr) {
        if (val == 1) totalOnes++;
    }
    if (totalOnes == 0) return 0;
    
    int currentOnes = 0, maxOnes = 0;
    for (int i = 0; i < totalOnes; i++) {
        if (arr[i] == 1) currentOnes++;
    }
    maxOnes = currentOnes;
    
    for (int i = totalOnes; i < arr.size(); i++) {
        if (arr[i] == 1) currentOnes++;
        if (arr[i - totalOnes] == 1) currentOnes--;
        maxOnes = max(maxOnes, currentOnes);
    }
    return totalOnes - maxOnes;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- array containing absolutely no 1s, array composed entirely of 1s, deeply alternating 1s and 0s.

NEXT: [[Index]]
