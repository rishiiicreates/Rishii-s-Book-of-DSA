# Count with given XOR

## Problem Statement
- given an array of integers and an integer `B`, find the number of subarrays whose bitwise XOR evaluates to `B`.

## Approach / Intuition
- we can solve this by adapting the standard [[Prefix Sum]] technique for bitwise XOR. If a subarray from index `i` to `j` has an XOR of `B`, then the XOR prefix up to `j` and the XOR prefix up to `i-1` must XOR to `B`. Mathematically, `prefix_xor[j] ^ prefix_xor[i-1] == B` implies `prefix_xor[i-1] == prefix_xor[j] ^ B`. We can maintain a hash map of prefix XOR frequencies as we traverse the array to count the occurrences of `prefix_xor[j] ^ B` in $O(1)$ expected time.

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <unordered_map>

using namespace std;

int solve(vector<int>& A, int B) {
    unordered_map<int, int> freq;
    int count = 0;
    int curr_xor = 0;
    
    freq[0] = 1;
    
    for (int num : A) {
        curr_xor ^= num;
        
        int target = curr_xor ^ B;
        if (freq.count(target)) {
            count += freq[target];
        }
        
        freq[curr_xor]++;
    }
    
    return count;
}
```

## New Keywords / STL Used
- `std::unordered_map`

## Edge Cases
- empty array
- `b = 0` requiring extra care since subarrays with 0 XOR count when current prefix equals a past prefix
- entire array XOR evaluates to `B`

NEXT: [[Index]]
