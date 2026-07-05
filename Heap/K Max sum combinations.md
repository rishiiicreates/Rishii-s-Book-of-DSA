# K Max sum combinations

## Problem Statement
- given two equally sized arrays (`A` and `B`), find the `K` maximum sum combinations `(A[i] + B[j])` that can be formed.

## Approach / Intuition
- first, sort both arrays in descending order. The largest possible sum is clearly `A[0] + B[0]`. We use a max-[[Heap]] (priority queue) to store pairs of `(sum, (indexA, indexB))` and a set to keep track of visited index pairs so we don't process them multiple times. Start by pushing `(A[0]+B[0], (0, 0))`. Extract the max from the heap `K` times. For each extracted element `(sum, (i, j))`, try adding `(A[i+1]+B[j], (i+1, j))` and `(A[i]+B[j+1], (i, j+1))` to the heap if they haven't been visited.

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N + K log K)
- **[[space Complexity]]:** O(K) for the heap and visited set.

## Sample Code
```cpp
#include <vector>
#include <queue>
#include <set>
#include <algorithm>

using namespace std;

vector<int> kMaxSumCombination(vector<int>& A, vector<int>& B, int K) {
    sort(A.begin(), A.end(), greater<int>());
    sort(B.begin(), B.end(), greater<int>());
    
    priority_queue<pair<int, pair<int, int>>> pq;
    set<pair<int, int>> visited;
    
    pq.push({A[0] + B[0], {0, 0}});
    visited.insert({0, 0});
    
    vector<int> result;
    
    for (int count = 0; count < K; count++) {
        auto top = pq.top();
        pq.pop();
        
        result.push_back(top.first);
        int i = top.second.first;
        int j = top.second.second;
        
        if (i + 1 < A.size() && visited.find({i + 1, j}) == visited.end()) {
            pq.push({A[i + 1] + B[j], {i + 1, j}});
            visited.insert({i + 1, j});
        }
        
        if (j + 1 < B.size() && visited.find({i, j + 1}) == visited.end()) {
            pq.push({A[i] + B[j + 1], {i, j + 1}});
            visited.insert({i, j + 1});
        }
    }
    
    return result;
}
```

## New Keywords / STL Used
- `std::priority_queue`, `std::set`, `std::pair`, `std::greater`

## Edge Cases
- `k` is equal to `N*N`.
- arrays contain negative numbers.

NEXT: [[Index]]
