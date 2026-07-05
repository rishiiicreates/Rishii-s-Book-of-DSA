# 3 Sum (Zero Sum Triplets)

## Problem Statement
- given an integer array $A$ of size $N$, return all mathematically unique triplets $[A[i], A[j], A[k]]$ such that $i, j, k$ are distinct and $A[i] + A[j] + A[k] = 0$. The solution set must not contain duplicate triplets.

- *example:* $A = [-1, 0, 1, 2, -1, -4]$
- *result:* $[[-1, -1, 2], [-1, 0, 1]]$


## Approach: Fix, Find, and Deduplicate

- this extends the standard [[Triplet Sum]] logic but introduces a severe mathematical constraint: preventing duplicate triplets in the output without relying on a slow, memory-intensive `std::set`.

- **sort:** Sorting establishes monotonicity for [[Two-Pointer]] convergence and naturally groups identical elements adjacently, enabling constant-time deduplication.
- **fixation with Deduplication:** Iterate `i` from $0$ to $N-3$.
   - *deduplication Rule 1:* If $i > 0$ and $A[i] == A[i-1]$, we skip `i`. We have already explored all triplets where this value acts as the smallest element.
   - *early Exit Optimization:* Since the array is sorted, if $A[i] > 0$, the sum can never mathematically reach $0$ (all subsequent elements are positive). We can break immediately.
- **convergence Search:** Set `left = i + 1` and `right = N - 1`. If $S = A[i] + A[\text{left}] + A[\text{right}] == 0$, we record the triplet.
- **pointer Deduplication:** After finding a valid triplet, we must advance `left` and `right`. To prevent generating the exact same triplet again, we mathematically skip over adjacent duplicates:
   - *deduplication Rule 2:* `while (left < right && A[left] == A[left+1]) left++;`
   - *deduplication Rule 3:* `while (left < right && A[right] == A[right-1]) right--;`
   - finally, explicitly step both pointers inward once more.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> res;
    int n = nums.size();
    if (n < 3) return res;
    
    sort(nums.begin(), nums.end());
    
    for (int i = 0; i < n - 2; ++i) {
        // Optimization: Smallest element is positive, impossible to sum to 0
        if (nums[i] > 0) break;
        
        // Deduplication 1: Skip duplicate `i`
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        
        int left = i + 1;
        int right = n - 1;
        
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            
            if (sum == 0) {
                res.push_back({nums[i], nums[left], nums[right]});
                
                // Deduplication 2: Skip duplicate `left`
                while (left < right && nums[left] == nums[left + 1]) left++;
                // Deduplication 3: Skip duplicate `right`
                while (left < right && nums[right] == nums[right - 1]) right--;
                
                // Advance past the duplicates
                left++;
                right--;
            } else if (sum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    
    return res;
}
```

> [!warning]
> Forgetting any of the three deduplication rules will result in duplicate output vectors. Utilizing a `set<vector<int>>` to handle duplicates lazily is considered an anti-pattern due to the massive logarithmic overhead of inserting vectors into a red-black tree.


## Complexity Analysis
- **time Complexity:** $O(N^2)$. The sorting is $O(N \log N)$. The nested loop structure takes precisely $O(N^2)$ time.
- **space Complexity:** $O(1)$ auxiliary space, excluding the memory allocated for the `res` output matrix.

NEXT: [[Index]]
