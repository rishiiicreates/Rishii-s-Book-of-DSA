# XOR of a given range

## Problem Statement
- find the bitwise XOR of elements in a range $[L, R]$.

## Approach / Intuition
- the XOR operation and its inverse are the same. We can compute the [[Prefix Sum]] (Prefix XOR) of the array. The XOR of range $[L, R]$ can then be found in $O(1)$ time by evaluating `prefix[R] ^ prefix[L - 1]`.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ preprocessing, $O(1)$ per query
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>

using namespace std;

class PrefixXOR {
    vector<int> pref;

public:
    PrefixXOR(const vector<int>& arr) {
        int n = arr.size();
        pref.resize(n + 1, 0);
        for (int i = 0; i < n; i++) {
            pref[i + 1] = pref[i] ^ arr[i];
        }
    }

    int query(int l, int r) {
        return pref[r + 1] ^ pref[l];
    }
};
```

## New Keywords / STL Used
- bitwise XOR operator `^`

## Edge Cases
- range starts at index 0, range of length 1, array of all zeroes.

NEXT: [[Index]]
