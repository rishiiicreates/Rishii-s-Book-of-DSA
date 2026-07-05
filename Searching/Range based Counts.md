# Range Based Counts

## Problem Statement
- given a sorted array $A$ of $N$ integers, answer queries requesting the total count of elements that fall strictly within an inclusive range $[L, R]$.

- *example:* $A = [1, 2, 4, 4, 5, 8, 10]$, Query: $[3, 8]$
- *result:* $4$ (Elements are $4, 4, 5, 8$)


## Approach: Lower Bound & Upper Bound

- because the array is sorted, we don't need to traverse it linearly ($O(N)$). We can locate the boundaries of the range $[L, R]$ using two [[Binary Search]] calls.

- **start Boundary:** We need the index of the first element $\ge L$. This is strictly the mathematical `lower_bound`.
- **end Boundary:** We need the index of the first element $> R$. This is strictly the mathematical `upper_bound`.
- the count of elements falling in $[L, R]$ is precisely the difference between the two indices.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int countInRange(const vector<int>& arr, int L, int R) {
    // Find the first element >= L
    auto startIt = lower_bound(arr.begin(), arr.end(), L);
    
    // Find the first element > R
    auto endIt = upper_bound(arr.begin(), arr.end(), R);
    
    // The number of elements is the distance between the two iterators
    return distance(startIt, endIt);
}
```

> [!tip]
> In C++, `std::distance(it1, it2)` for random-access iterators (like `std::vector` iterators) executes in $O(1)$ time by simply computing `it2 - it1`.

> [!warning]
> If $L > R$, the result will safely be $0$ assuming $L$ and $R$ bounds don't incorrectly overlap the lower and upper bounds, but mathematically `startIt` will be $\ge$ `endIt`, yielding $\le 0$ distance. It's a good practice to manually return $0$ if $L > R$.


## Complexity Analysis
- **time Complexity:** $O(\log_2 N)$ per query. Both `lower_bound` and `upper_bound` run in logarithmic time.
- **space Complexity:** $O(1)$ auxiliary space.

NEXT: [[Index]]
