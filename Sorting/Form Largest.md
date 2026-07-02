---
type: concept
tags: [sorting, cpp, custom-comparator]
date: 2026-06-30
---
# Form Largest

## Problem Statement
Given a list of non-negative integers $A$, arrange them such that they form the largest possible concatenated number. Since the result may be very large, return it as a string.

*Example:* $A = [3, 30, 34, 5, 9]$
*Result:* `"9534330"`

---

## Approach: Custom Comparator Sorting

To maximize the concatenated string, we must sort the numbers using a [[Custom Comparator]]. Instead of sorting purely by numerical or lexicographical order, we want to know for any two strings $S_1$ and $S_2$, whether placing $S_1$ before $S_2$ yields a larger combined string than placing $S_2$ before $S_1$.

1. Convert all integers to strings.
2. Sort the strings using a custom comparator. The comparator evaluates `a + b > b + a`. If concatenating $a$ then $b$ forms a strictly larger number (as a string) than $b$ then $a$, $a$ should come first.
3. If the largest element after sorting is `"0"`, the entire result is just `"0"` (to avoid outputs like `"000"`).
4. Concatenate the sorted strings and return.

---

## Code Implementation

```cpp
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

string largestNumber(const vector<int>& nums) {
    vector<string> strs;
    strs.reserve(nums.size());
    for (int num : nums) {
        strs.push_back(to_string(num));
    }
    
    // Sort using custom lambda comparator
    sort(strs.begin(), strs.end(), [](const string& a, const string& b) {
        return a + b > b + a;
    });
    
    // Edge case: all zeroes
    if (strs.front() == "0") {
        return "0";
    }
    
    string res;
    for (const string& s : strs) {
        res += s;
    }
    
    return res;
}
```

> [!important]
> The custom comparator mathematically acts as a transitive relation: if $A \cdot B > B \cdot A$ and $B \cdot C > C \cdot B$, then $A \cdot C > C \cdot A$. This guarantees that a stable topological ordering exists.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N \cdot L)$, where $N$ is the number of elements and $L$ is the maximum length of a string. The string concatenation and comparison in the comparator takes $O(L)$ time.
- **Space Complexity:** $O(N \cdot L)$ to store the array of strings and the final concatenated result.
