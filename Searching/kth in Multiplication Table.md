# Kth Smallest Number in Multiplication Table

## Problem Statement
- given the height $M$ and the length $N$ of a multiplication table, find the $K$-th smallest number in this $M \times N$ multiplication table.
- the table is 1-indexed, meaning the cell at row $i$ and column $j$ contains the value $i \times j$.


## Approach: Parametric Binary Search on Count Space

- generating the entire table and sorting it takes $\mathcal{O}(M \times N \log(M \times N))$, which will TLE for large bounds ($M, N \le 3 \times 10^4$).
- instead of searching indices, we **binary search the actual value space** from $1$ to $M \times N$.

- the key mathematical observation: For any integer $X$, how many numbers in the multiplication table are $\le X$?
- in the $i$-th row, the numbers are $i, 2i, 3i, \dots, N \times i$.
- the number of multiples of $i$ that are $\le X$ is exactly $\lfloor X / i \rfloor$.
- however, since there are only $N$ columns, the count in row $i$ is strictly bounded by $\min(\lfloor X / i \rfloor, N)$.

- the count function $C(X) = \sum_{i=1}^{M} \min(\lfloor X / i \rfloor, N)$ is monotonically increasing.
- we use Binary Search to find the **smallest** $X$ such that $C(X) \ge K$.


## Code Implementation

```cpp
#include <algorithm>
using namespace std;

int countLessEqual(int mid, int m, int n) {
    int count = 0;
    // Iterate through each row and count valid multiples
    for (int i = 1; i <= m; i++) {
        count += min(mid / i, n);
    }
    return count;
}

int findKthNumber(int m, int n, int k) {
    int low = 1;
    int high = m * n;
    int ans = -1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (countLessEqual(mid, m, n) >= k) {
            ans = mid;
            high = mid - 1; // Try to find a smaller valid number
        } else {
            low = mid + 1;
        }
    }
    
    return ans;
}
```

> [!tip]
> You might wonder: "What if the `ans` found by binary search isn't actually a number in the multiplication table?"
> Mathematics guarantees it will be. If `mid` is not in the table, then $C(\text{mid}) == C(\text{mid}-1)$. The binary search will aggressively push `high` downwards until it hits the exact value that caused the count to jump to $\ge K$, which inherently must be a valid product in the table.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(M \log(M \times N))$. The binary search runs $\log(M \times N)$ times. In each iteration, counting elements takes $\mathcal{O}(M)$ time since we iterate through all rows.
- **space Complexity:** $\mathcal{O}(1)$. The evaluation relies strictly on mathematical calculation without auxiliary structures.

NEXT: [[Index]]
