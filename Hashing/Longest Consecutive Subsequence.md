---
type: concept
tags: [hashing, cpp, arrays]
date: 2026-06-30
---
# Longest Consecutive Sequence

## Problem Statement
Given an unsorted array $A$ of integers, return the length of the longest consecutive elements sequence. The algorithm must run in strictly $O(N)$ time.

*Example:* $A = [100, 4, 200, 1, 3, 2]$
*Result:* $4$ (The sequence is $[1, 2, 3, 4]$)

---

## Approach: Hash Set Boundary Identification

A naive sorting approach takes $O(N \log N)$. To achieve $O(N)$ time, we leverage a [[Hash Set]] for $O(1)$ lookups. The mathematical insight is to identify the *boundary* elements of consecutive sequences.

1. Insert all elements of $A$ into a Hash Set. This removes duplicates and allows $O(1)$ structural queries.
2. Iterate through each element $x$ in the Hash Set.
3. We only want to begin counting a sequence if $x$ is the absolute minimum (the start) of that sequence. We can mathematically verify this by checking if $x - 1$ exists in the set.
   - If $x - 1 \in \text{Set}$, $x$ is *not* the start of a sequence. We skip it.
   - If $x - 1 \notin \text{Set}$, $x$ *is* the start. We now iteratively query for $x + 1, x + 2 \dots$ until the sequence breaks, tracking the `current_streak`.
4. Maintain a running maximum of the streak lengths. By only tracing forward from the lowest bounds, every element is visited inside the inner loop exactly once globally.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_set>
#include <algorithm>
using namespace std;

int longestConsecutive(const vector<int>& nums) {
    if (nums.empty()) return 0;
    
    unordered_set<int> num_set(nums.begin(), nums.end());
    int longest_streak = 0;
    
    for (int num : num_set) {
        // Only initiate sequence tracking if `num` is a mathematically valid sequence start boundary
        if (num_set.find(num - 1) == num_set.end()) {
            int current_num = num;
            int current_streak = 1;
            
            // Incrementally probe the hash set for consecutive elements
            while (num_set.find(current_num + 1) != num_set.end()) {
                current_num++;
                current_streak++;
            }
            
            longest_streak = max(longest_streak, current_streak);
        }
    }
    
    return longest_streak;
}
```

> [!important]
> The condition `num_set.find(num - 1) == num_set.end()` guarantees that the inner `while` loop runs at most $N$ times *across the entire execution of the algorithm*, strictly enforcing the $O(N)$ time bound despite the nested loops.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. Building the set takes $O(N)$. The outer loop processes each of the $N$ elements, but the inner `while` loop only executes for elements that are the start of a sequence. Thus, each element is queried at most twice.
- **Space Complexity:** $O(N)$ to store the array elements in the `unordered_set`.
