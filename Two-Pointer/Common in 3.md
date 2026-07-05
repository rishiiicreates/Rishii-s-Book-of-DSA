# Find Common Elements in Three Sorted Arrays

## Problem Statement
- given three sorted arrays, find all the common elements across all three arrays.

## Approach / Intuition
- use three pointers, one for each array. Compare the elements at these pointers. If all three are equal, add to the result and advance all pointers. Otherwise, find the array with the smallest current element and advance its pointer. This is an extension of the [[Two Pointers]] intersection logic to three arrays.

## Time & Space Complexity
- **[[time Complexity]]:** O(n1 + n2 + n3)
- **[[space Complexity]]:** O(1) excluding output space

## Sample Code
```cpp
vector<int> commonElements(const vector<int>& a, const vector<int>& b, const vector<int>& c) {
    int i = 0, j = 0, k = 0;
    vector<int> res;
    while (i < a.size() && j < b.size() && k < c.size()) {
        if (i > 0 && a[i] == a[i - 1]) { i++; continue; }
        if (j > 0 && b[j] == b[j - 1]) { j++; continue; }
        if (k > 0 && c[k] == c[k - 1]) { k++; continue; }
        
        if (a[i] == b[j] && b[j] == c[k]) {
            res.push_back(a[i]);
            i++; j++; k++;
        } else if (a[i] < b[j]) {
            i++;
        } else if (b[j] < c[k]) {
            j++;
        } else {
            k++;
        }
    }
    return res;
}
```

## New Keywords / STL Used
- `continue`

## Edge Cases
- no common elements, one or more empty arrays, elements appear multiple times but not across all arrays.

NEXT: [[Index]]
