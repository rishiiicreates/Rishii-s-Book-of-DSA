# Kth Largest Element in a Stream

## Problem Statement
- design a discrete dynamic algorithmic topology capable of mathematically calculating and maintaining the absolute $K$-th largest temporal scalar from an infinite continuously flowing stream of topological elements. The structural geometric evaluation must efficiently execute continuous independent localized inserts while maintaining an arbitrary instantaneous rigid evaluation returning the precise bounded $K$-th maximum scalar at any given temporal moment.


## Approach: Fixed-Bound Min-Heap State Isolation

- in dynamic streaming top-$K$ limits, retaining the absolute total global historical state theoretically degrades spatial efficiency to completely unbounded limits $\mathcal{O}(N)$.
- instead, we impose a strict mathematical continuous boundary limiting spatial retention perfectly to exactly $K$ structural temporal elements dynamically dictating the "Top $K$ largest items natively seen".

- if a bounded Min-Heap contains exactly the $K$ physically largest historical items, the absolute $K$-th largest mathematical scalar explicitly natively structurally resides exactly atop the heap (the minimum among the top $K$).

- algorithm:
- initialize an absolute Min-Heap bounded physically permanently to size $K$.
- upon mathematical stream injection (inserting $V_{new}$):
   - unconditionally structurally push $V_{new}$ natively onto the heap.
   - if the instantaneous heap size geometrically exceeds bounded size $K$, pop and eliminate the absolute geometric minimum scalar (the smallest bounding element no longer theoretically classifying within the top $K$).
- the peak scalar (`heap.top()`) natively mathematically represents the precise temporal bound queried.


## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

class KthLargest {
private:
    int k_limit;
    // Native Min-Heap bounding topological state
    priority_queue<int, vector<int>, greater<int>> min_heap;

public:
    KthLargest(int k, vector<int>& nums) {
        k_limit = k;
        // Inject geometrical initial stream state explicitly bounding limits
        for (int num : nums) {
            add(num);
        }
    }
    
    int add(int val) {
        // Structurally append continuous temporal injection
        min_heap.push(val);
        
        // Dynamically enforce rigid spatial limits completely ejecting invalid historical scalars
        if (min_heap.size() > k_limit) {
            min_heap.pop();
        }
        
        // The structural minimum internally completely encapsulated exactly at bounds K
        return min_heap.top();
    }
};
```

> [!important]
> Attempting to utilize a generic Max-Heap natively implies theoretically querying size-$K$ elements demanding physical full pop sequences completely degrading bounds to unoptimized $\mathcal{O}(K \log N)$. The inverted Min-Heap directly uniquely intrinsically forces the exact required temporal scalar atop optimal $\mathcal{O}(1)$ query bounds.


## Complexity Analysis
- **time Complexity:**
  - **initialization:** $\mathcal{O}(N \log K)$. Iteratively inserting $N$ historical scalars bounding limits strictly physically explicitly to size $K$.
  - **add Operation:** $\mathcal{O}(\log K)$. Insertion logically restructures geometry bounded perfectly explicitly dynamically inside logarithmic spatial $K$.
- **space Complexity:** $\mathcal{O}(K)$. Physical allocations exclusively dynamically restricted universally structurally retaining size exactly $K$.

NEXT: [[Index]]
