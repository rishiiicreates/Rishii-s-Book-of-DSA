# Generate All Subarrays

## Problem Statement
- given an array of size $N$, generate and print all possible subarrays. A subarray is a contiguous sequence of elements within an array.


## Approach: Combinatorial Bounding

- from a combinatorial perspective, a subarray is uniquely defined by choosing two indices: a starting index $i$ and an ending index $j$, where $0 \le i \le j < N$.
- the total number of such subarrays for an array of size $N$ is exactly the number of ways to choose 2 distinct points from $N+1$ points (acting as boundaries), which is mathematically given by $\binom{N+1}{2} = \frac{N(N+1)}{2}$.

- to generate them, we utilize a nested boundary selection strategy:
- the **outermost loop** fixes the starting index $i$.
- the **middle loop** fixes the ending index $j$ (ensuring $j \ge i$).
- the **innermost loop** iterates from $i$ to $j$ to extract and print the contiguous segment.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

void generateSubarrays(const vector<int>& arr) {
    int n = arr.size();
    
    // i represents the starting index
    for (int i = 0; i < n; ++i) {
        
        // j represents the ending index
        for (int j = i; j < n; ++j) {
            
            // k extracts the elements bounded by i and j
            for (int k = i; k <= j; ++k) {
                cout << arr[k] << " ";
            }
            cout << endl; // Move to next line for next subarray
        }
    }
}
```

> [!warning]
> The brute-force generation of subarrays is fundamentally slow because there are $\mathcal{O}(N^2)$ subarrays, and printing/accumulating each takes $\mathcal{O}(N)$ on average. Do not use this for $N > 10^3$ in competitive programming or latency-critical applications.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N^3)$. The number of subarrays is $\frac{N(N+1)}{2}$, and the average length of a subarray is $\frac{N}{3}$. Thus, printing all of them requires approximately $\frac{N^3}{6}$ operations.
- **space Complexity:** $\mathcal{O}(1)$. The algorithm only requires loop iterators and does not dynamically allocate extra memory structure.

NEXT: [[Index]]
