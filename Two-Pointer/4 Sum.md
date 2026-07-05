# 4 Sum (Quadruplet Sum to Target)

## Problem Statement
- given an array of $N$ integers, mathematically identify all strictly unique quadruplets $(a, b, c, d)$ such that their algebraic sum evaluates exactly to a target integer $T$:
$$a + b + c + d = T$$


## Approach: Nested Iteration with Two-Pointer Convergence

- a brute-force mathematical evaluation requires combinatorial $\mathcal{O}(N^4)$ time. By algebraically reducing the problem, we can decouple the quadruplet.
- strictly sort the array in $\mathcal{O}(N \log N)$ to cluster duplicates and enable monotonic traversal.
- fix the first mathematical variable $a$ using an outer loop `i` $\in [0, N-4]$.
- fix the second mathematical variable $b$ using an inner loop `j` $\in [i+1, N-3]$.
- the structural requirement geometrically reduces to finding $(c, d)$ such that:
   $$c + d = T - (a + b)$$
- this reduced equation is a standard 2-Sum problem on a strictly sorted sequence, solvable in $\mathcal{O}(N)$ using a converging [[Two-Pointer]] technique (`left = j+1`, `right = N-1`).
- **strict Deduplication:** To prevent mathematically redundant combinations, strictly skip identical adjacent elements when incrementing `i`, `j`, `left`, and `right`.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

vector<vector<int>> fourSum(vector<int>& nums, int target) {
    int n = nums.size();
    vector<vector<int>> res;
    if (n < 4) return res;
    
    // Sort to enable monotonicity and clustering
    sort(nums.begin(), nums.end());
    
    for (int i = 0; i < n - 3; i++) {
        // Mathematical deduplication for a
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        
        for (int j = i + 1; j < n - 2; j++) {
            // Mathematical deduplication for b
            if (j > i + 1 && nums[j] == nums[j - 1]) continue;
            
            int left = j + 1;
            int right = n - 1;
            
            while (left < right) {
                // Mandate 1LL cast to strictly prevent 32-bit integer overflow
                long long sum = 1LL * nums[i] + nums[j] + nums[left] + nums[right];
                
                if (sum == target) {
                    res.push_back({nums[i], nums[j], nums[left], nums[right]});
                    
                    // Deduplication for c and d
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;
                    
                    left++;
                    right--;
                } 
                else if (sum < target) left++;
                else right--;
            }
        }
    }
    
    return res;
}
```

> [!warning]
> The accumulation `nums[i] + nums[j] + nums[left] + nums[right]` can mathematically exceed the signed 32-bit integer limit `2147483647`. You must strictly upcast the initial operand using `1LL * nums[i]` or explicitly cast `(long long)nums[i]` before executing the addition sequence to prevent silent structural overflow.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N^3)$. Fixing `i` and `j` requires $\mathcal{O}(N^2)$ iterations, and the nested two-pointer convergence scans strictly $\mathcal{O}(N)$ elements per `(i, j)` state.
- **space Complexity:** $\mathcal{O}(1)$ auxiliary structural memory. The sorting algorithm (Introsort) may internally utilize $\mathcal{O}(\log N)$ stack space.

NEXT: [[Index]]
