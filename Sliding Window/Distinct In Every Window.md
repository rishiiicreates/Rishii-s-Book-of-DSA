# Distinct In Every Window

## Problem Statement
- given an array of integers and a number `K`, find the count of distinct elements in every contiguous window of size `K`.

## Approach / Intuition
- maintain a hash map to continuously track the frequency of elements within the active window. As the window shifts right, increment the frequency of the incoming element and decrement the outgoing element's frequency. If an element's frequency drops to zero, physically erase it from the map so the hash map size always accurately reflects the distinct count, combining [[Sliding Window]] with dynamic [[Hash Map Tracking]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(K)

## Sample Code
```cpp
vector<int> countDistinct(vector<int>& arr, int k) {
    vector<int> res;
    if (k > arr.size()) return res;
    unordered_map<int, int> freq;
    for (int i = 0; i < k; i++) freq[arr[i]]++;
    res.push_back(freq.size());
    for (int i = k; i < arr.size(); i++) {
        freq[arr[i]]++;
        freq[arr[i - k]]--;
        if (freq[arr[i - k]] == 0) freq.erase(arr[i - k]);
        res.push_back(freq.size());
    }
    return res;
}
```

## New Keywords / STL Used
- `std::unordered_map`

## Edge Cases
- `k = 1` (yielding output array of 1s), `K = N`, identical elements populating the window.

NEXT: [[Index]]
