---
type: concept
tags: [hashing, cpp, hash-set]
date: 2026-06-30
---
# Count Distinct Elements in an Array

## Problem Statement
Given an array $A$ of $N$ integers, return the number of mathematically distinct elements.

*Example:* $A = [10, 20, 10, 20, 30]$
*Result:* $3$ (Distinct elements are $10, 20, 30$)

---

## Approach: Hash Set

A mathematical Set is a collection of unique objects. By iterating through the array and inserting every element into a [[Hash Set]], duplicates are naturally absorbed (rejected), leaving only distinct elements.

1. Initialize an empty `std::unordered_set`.
2. Iterate through $A$, inserting $A[i]$ into the set.
3. The size of the set at the end of the iteration is exactly the number of distinct elements.
4. *Optimization:* In C++, we can use the range constructor of `std::unordered_set` to do this elegantly in a single line.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_set>
using namespace std;

int countDistinct(const vector<int>& arr) {
    // Constructing the set directly from the array iterators
    unordered_set<int> s(arr.begin(), arr.end());
    
    return s.size();
}
```

> [!tip]
> While a Hash Set runs in expected $O(N)$ time, its heavy constant factors (memory allocation, hash function overhead) can sometimes make it slower than simply sorting the array ($O(N \log N)$) and counting unique adjacent elements, especially for small $N$ or small data types where CPU cache locality dominates.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ expected. Inserting into an `unordered_set` takes $O(1)$ on average. Worst case is $O(N^2)$ if the hash function causes catastrophic collisions (though rare with modern STL).
- **Space Complexity:** $O(N)$ to store up to $N$ unique elements in the hash set.
