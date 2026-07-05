# Book Allocation

## Problem Statement
- given an array $A$ of $N$ integers where $A[i]$ denotes the number of pages in the $i$-th book, and an integer $M$ denoting the number of students. Allocate all the books to the students such that:
- each student gets at least one book.
- each student is assigned a contiguous sequence of books.
- the maximum number of pages assigned to a student is minimized.

- return the minimum possible maximum pages. If allocation is not possible, return $-1$.

- *example:* $A = [12, 34, 67, 90]$, $M = 2$
- *result:* $113$ (Student 1: $12, 34, 67$, Student 2: $90$)


## Approach: Binary Search on the Answer

- this is a classic "min-max" optimization problem. The optimal maximum pages a student can read, let's call it $X$, must lie in a predictable range:
- **lower Bound ($L$):** The maximum pages in a single book. (Since every book must be read, a student assigned the thickest book must read at least this many pages). $L = \max(A)$
- **upper Bound ($R$):** The sum of all pages. (If there is only $1$ student, they must read everything). $R = \sum_{i} A[i]$

- we can use [[Binary Search]] to find the optimal $X$. For a given candidate `mid` (representing the maximum allowed pages per student):
- traverse the array greedily. Maintain a running sum of pages for the current student.
- if adding the next book exceeds `mid`, assign the book to a new student.
- if the total number of students required exceeds $M$, then `mid` is too small. We search the right half: `low = mid + 1`.
- if we can allocate within $\le M$ students, `mid` is a valid configuration. We record it as a potential answer and try to find a smaller maximum by searching the left half: `high = mid - 1`.


## Code Implementation

```cpp
#include <vector>
#include <numeric>
#include <algorithm>
using namespace std;

bool isPossible(const vector<int>& A, int M, int mid) {
    int studentCount = 1;
    int pageSum = 0;
    
    for (int pages : A) {
        if (pageSum + pages <= mid) {
            pageSum += pages;
        } else {
            studentCount++;
            if (studentCount > M || pages > mid) {
                return false;
            }
            pageSum = pages;
        }
    }
    return true;
}

int allocateBooks(const vector<int>& A, int M) {
    if (M > A.size()) return -1; // Not enough books
    
    int low = *max_element(A.begin(), A.end());
    int high = accumulate(A.begin(), A.end(), 0);
    int ans = -1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (isPossible(A, M, mid)) {
            ans = mid;
            high = mid - 1; // Try to minimize further
        } else {
            low = mid + 1;  // Increase the limit
        }
    }
    
    return ans;
}
```

> [!tip]
> This pattern applies to many "divide array into K contiguous subarrays such that the maximum subarray sum is minimized" problems (e.g., Painter's Partition, Split Array Largest Sum).


## Complexity Analysis
- **time Complexity:** $O(N \log_2(\sum A[i] - \max A[i]))$. The search space is bounded by the sum of elements, and for each binary search step, we do a linear scan $O(N)$ of the array.
- **space Complexity:** $O(1)$. No auxiliary space used.

NEXT: [[Index]]
