---
type: concept
tags: [sorting, cpp, custom-comparator, stable-sort]
date: 2026-07-01
---
# Sort by Length

## Problem Statement
Given an array of strings $S$ of size $N$, sort them in ascending order of their mathematical length. If two strings possess the exact same length (i.e., $|S[i]| == |S[j]|$), they must absolutely maintain their original relative order in the output.

---

## Approach: Stable Custom Comparator

By default, string sorting algorithms compare strings lexicographically (dictionary order). To sort by a mathematical property of the object (its length), we must supply a **Custom Comparator**.

A comparator is a strict weak ordering function $f(A, B)$ that returns `true` if $A$ must appear strictly before $B$.
For our constraints:
$$ f(A, B) = |A| < |B| $$

Furthermore, the problem dictates that relative order must be preserved for elements evaluated as equivalent by the comparator ($|A| == |B|$). The standard `std::sort` implements Introsort, which is mathematically unstable (it freely swaps equivalent elements).
We must enforce the use of `std::stable_sort`, which implements Merge Sort and guarantees relative order preservation.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

void sortByLength(vector<string>& arr) {
    // std::stable_sort guarantees preservation of original relative order
    // for strings mapping to the exact same length constraint.
    stable_sort(arr.begin(), arr.end(), [](const string& a, const string& b) {
        return a.length() < b.length();
    });
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N \log^2 N)$ normally, but $O(N \log N)$ if sufficient memory is available. `std::stable_sort` relies heavily on memory allocations.
- **Space Complexity:** $O(N)$ extra memory bound required by Merge Sort to guarantee stability.

> [!warning]
> **Comparator Rules:** A C++ comparator must model a *strict weak ordering*. It must return `false` if the two elements are equivalent. NEVER write a comparator like `return a.length() <= b.length()`. The `<=` operator violates irreflexivity and will cause `std::sort` to out-of-bounds segfault in certain compilers.
