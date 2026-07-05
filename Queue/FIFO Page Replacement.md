# FIFO Page Replacement

## Problem Statement
- given a discrete stream of memory Page IDs and a mathematically bounded physical frame cache of size $K$, simulate the First-In-First-Out (FIFO) page replacement protocol. If a requested page is not in the cache, a Page Fault occurs, and the absolute oldest page in the cache must be topologically evicted to store the new page. Compute total Page Faults.


## Approach: Queue Tracking with Hash State

- the FIFO eviction protocol is perfectly isomorphic to the Queue ADT. The frame cache acts as a bounded sliding queue.
- to strictly enforce mathematical set uniqueness (avoiding $O(K)$ linear scans on every page request), we augment the Queue with a boolean state array or Hash Set.

- **state Machine:**
- let $Q$ be the eviction tracking Queue, and $H$ be the active inclusion Set.
- for each requested Page $P_i$ in the stream:
- **cache Hit Verification:** If $P_i \in H$, no operation is required (the page is already structurally mapped).
- **page Fault ($P_i \notin H$):**
   - increment Fault Counter.
   - **eviction Phase:** If the structural bound $|Q| == K$, we must mathematically eject a page. We pop the element $P_e$ from the Front of $Q$ and delete $P_e$ from $H$.
   - **injection Phase:** Enqueue $P_i$ into $Q$, and insert $P_i$ into $H$.


## Code Implementation

```cpp
#include <vector>
#include <queue>
#include <unordered_set>

using namespace std;

int pageFaults(int N, int K, vector<int>& pages) {
    unordered_set<int> cache_set; // O(1) mathematical lookup mapping
    queue<int> eviction_queue;    // FIFO chronological topology
    
    int fault_count = 0;
    
    for (int i = 0; i < N; i++) {
        int current_page = pages[i];
        
        // Structural Cache Miss (Page Fault)
        if (cache_set.find(current_page) == cache_set.end()) {
            fault_count++;
            
            // Check Capacity Constraint
            if (eviction_queue.size() == K) {
                // Execute Eviction
                int oldest_page = eviction_queue.front();
                eviction_queue.pop();
                cache_set.erase(oldest_page);
            }
            
            // Inject new mapping
            cache_set.insert(current_page);
            eviction_queue.push(current_page);
        }
    }
    
    return fault_count;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ linear mapping. The hash set insertion/deletion and queue bounds evaluate strictly in $O(1)$ amortized time.
- **space Complexity:** $O(K)$ bound to the strict physical capacity constraints for both the tracking queue and inclusion set.

> [!important]
> **Belady's Anomaly:** Mathematically, one would intuitively hypothesize that strictly increasing frame capacity $K$ must monotonically decrease Page Faults. However, the FIFO protocol violates this mathematical monotonicity. For certain discrete access streams, increasing $K$ structurally *increases* total Page Faults! This non-monotonic topological failure is known as **Belady's Anomaly**.

NEXT: [[Index]]
