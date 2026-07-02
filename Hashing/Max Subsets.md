---
type: concept
tags: [hashing, cpp, subset]
date: 2026-06-30
---
# Maximum Subset Size of Distinct Elements

## Problem Statement
Given an array $A$ of $N$ integers, find the maximum size of a subset of $A$ such that all elements in the subset are mathematically distinct.

*Example:* $A = [4, 2, 4, 3, 2, 5]$
*Result:* $4$ (A subset can be $\{4, 2, 3, 5\}$)

---

## Approach: Set Theory Mapping

A subset $S \subseteq A$ with strictly distinct elements maps mathematically to the unique elements of $A$. Therefore, the maximum size of such a subset is exactly the cardinality of the mathematical set representation of $A$. 

We can map the array into a [[Hash Set]]. Since a Hash Set by definition rejects duplicates, inserting all $N$ elements of $A$ into the set will automatically yield the collection of unique elements. The cardinality (size) of this Hash Set is our answer.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_set>
using namespace std;

int maxSubsetSize(const vector<int>& arr) {
    // Constructing a hash set directly from the vector's range
    unordered_set<int> unique_elements(arr.begin(), arr.end());
    
    // The cardinality of the set is the maximum subset size of distinct elements
    return unique_elements.size();
}
```

> [!tip]
> In C++, passing the iterator range `(arr.begin(), arr.end())` to the constructor of `std::unordered_set` is not only concise but is often optimized by the STL allocator compared to a manual `for` loop inserting elements one by one.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ expected time. Each insertion into the hash set takes $O(1)$ amortized time.
- **Space Complexity:** $O(N)$ worst-case auxiliary space to store all elements if all elements in the array are originally distinct.
