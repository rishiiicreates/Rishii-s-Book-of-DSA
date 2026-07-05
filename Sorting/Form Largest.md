# Form Largest

## Problem Statement
- given a list of non-negative integers $A$, arrange them such that they form the largest possible concatenated number. Since the result may be very large, return it as a string.

- *example:* $A = [3, 30, 34, 5, 9]$
- *result:* `"9534330"`


## Approach: Custom Comparator Sorting

- to maximize the concatenated string, we must sort the numbers using a [[Custom Comparator]]. Instead of sorting purely by numerical or lexicographical order, we want to know for any two strings $S_1$ and $S_2$, whether placing $S_1$ before $S_2$ yields a larger combined string than placing $S_2$ before $S_1$.

- convert all integers to strings.
- sort the strings using a custom comparator. The comparator evaluates `a + b > b + a`. If concatenating $a$ then $b$ forms a strictly larger number (as a string) than $b$ then $a$, $a$ should come first.
- if the largest element after sorting is `"0"`, the entire result is just `"0"` (to avoid outputs like `"000"`).
- concatenate the sorted strings and return.


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


## Complexity Analysis
- **time Complexity:** $O(N \log N \cdot L)$, where $N$ is the number of elements and $L$ is the maximum length of a string. The string concatenation and comparison in the comparator takes $O(L)$ time.
- **space Complexity:** $O(N \cdot L)$ to store the array of strings and the final concatenated result.

NEXT: [[Index]]
