---
type: concept
tags: [sorting, cpp, greedy, two-pointers, sweep-line]
date: 2026-06-30
---
# Minimum Platforms Required

## Problem Statement
Given the arrival `arr[]` and departure `dep[]` times of all trains passing through a railway station, determine the strictly minimum number of platforms mathematically required so that no train is ever kept waiting.

---

## Approach: Sweep-Line Event Processing

This is a classic variation of the Line Sweep algorithm.
We are not mathematically concerned with *which* specific train occupies which platform. We are solely concerned with the overall **density of overlapping intervals** at any given infinitesimally small temporal point.

Algorithm:
1. Since train identities are irrelevant to the pure platform count, we completely decouple arrivals from departures.
2. Sort the `arr[]` array and the `dep[]` array independently.
3. Use a [[Two Pointers]] state machine (`i` for arrivals, `j` for departures).
4. If `arr[i] <= dep[j]`, a train is mathematically arriving before or exactly at the same time another leaves. The localized platform requirement increases by $1$. Increment `i`.
5. If `arr[i] > dep[j]`, a train has fully departed, freeing a platform. The localized platform requirement decreases by $1$. Increment `j`.
6. Track the absolute maximum of the localized requirement throughout the entire temporal scan.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int findPlatform(vector<int>& arr, vector<int>& dep) {
    // Decoupled deterministic sorting
    sort(arr.begin(), arr.end());
    sort(dep.begin(), dep.end());
    
    int n = arr.size();
    
    // Initial state: first train arrives
    int current_platforms = 1;
    int max_platforms = 1;
    
    int i = 1; // Start checking from 2nd arrival
    int j = 0; // Compare against 1st departure
    
    while (i < n && j < n) {
        if (arr[i] <= dep[j]) {
            current_platforms++;
            i++;
        } else {
            current_platforms--;
            j++;
        }
        
        // Track the absolute temporal maximum
        max_platforms = max(max_platforms, current_platforms);
    }
    
    return max_platforms;
}
```

> [!warning]
> The strict inequality `arr[i] <= dep[j]` handles the specific edge case where a train arrives at the *exact same minute* another departs. By standard Indian Railway metrics (and standard algorithmic bounds), the arriving train must be allocated a new platform, hence the equality dictates an increment before decrement.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log N)$ heavily dominated by the two independent sorting operations. The temporal sweep scan evaluates in strictly linear $\mathcal{O}(N)$ time.
- **Space Complexity:** $\mathcal{O}(\log N)$ auxiliary space required for the Introsort operations executed under `std::sort`.
