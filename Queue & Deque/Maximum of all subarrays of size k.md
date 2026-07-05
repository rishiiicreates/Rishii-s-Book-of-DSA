# Maximum of all subarrays of size k

## Problem Statement
- given an array and an integer `k`, find the maximum for each and every contiguous subarray of size `k`. (Note: This is a restatement of the Sliding Window Maximum problem, emphasizing the output nature).

## Approach / Intuition
- exactly the same as Sliding Window Maximum. We use a [[Deque]] to store indices of potential maximums. The deque is kept monotonically decreasing. Indices outside the current `k` window are removed from the front, and smaller elements are popped from the back before adding the new index.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(k)

## Sample Code
```cpp
vector<int> maxOfSubarrays(int *arr, int n, int k) {
    deque<int> dq;
    vector<int> res;
    for (int i = 0; i < n; i++) {
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }
        while (!dq.empty() && arr[dq.back()] < arr[i]) {
            dq.pop_back();
        }
        dq.push_back(i);
        if (i >= k - 1) {
            res.push_back(arr[dq.front()]);
        }
    }
    return res;
}
```

## New Keywords / STL Used
- `std::deque`

## Edge Cases
- `k=1`, array size equals `k`, array sorted in ascending or descending order.

NEXT: [[Index]]
