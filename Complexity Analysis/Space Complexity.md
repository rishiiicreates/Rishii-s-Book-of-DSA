---
type: concept
tags: [complexity, theory, space-complexity]
date: 2026-06-30
---
# Space Complexity

## What is Space Complexity?
Space complexity is a measure of the amount of working memory an algorithm needs to run, expressed as a function of the length of the input $n$. Like time complexity, it is evaluated asymptotically using Big-O notation.

> [!important]
> Space complexity encompasses **both** the memory required to hold the inputs and the extra working memory. However, when comparing algorithms, we usually focus on the **Auxiliary Space**—the temporary or extra space used by the algorithm, excluding the space taken by the inputs.

---

## Components of Space Complexity

Space complexity $S(n)$ typically consists of two parts:
1. **Fixed Part**: Memory that is independent of the input size. This includes instruction space (code), constants, simple variables, etc.
2. **Variable Part**: Memory whose size depends on the input size $n$. This includes dynamically allocated memory, large data structures (arrays, hash maps), and the recursion call stack.

$$S(n) = c + S_{\text{variable}}(n)$$

---

## Common Space Complexities

### $O(1)$ - Constant Space
The algorithm requires a fixed amount of extra space, regardless of the input size. These are often called **in-place** algorithms.
```cpp
// Reversing an array in-place uses O(1) auxiliary space
void reverseArray(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;
    while (left < right) {
        swap(arr[left], arr[right]); // Only a temporary variable is used
        left++; right--;
    }
}
```

### $O(n)$ - Linear Space
The memory required grows proportionally with the input size.
```cpp
// Allocating a new array of the same size requires O(n) space
vector<int> copyArray(const vector<int>& arr) {
    vector<int> newArr(arr.size());
    for (int i = 0; i < arr.size(); i++) {
        newArr[i] = arr[i];
    }
    return newArr;
}
```

### $O(n^2)$ - Quadratic Space
Typically seen when generating a 2D matrix, such as in Dynamic Programming or graph adjacency matrices.
```cpp
// Allocating a 2D grid
vector<vector<int>> createGrid(int n) {
    return vector<vector<int>>(n, vector<int>(n, 0));
}
```

---

## The Hidden Cost: Recursive Call Stack

A very common pitfall in evaluating space complexity is forgetting the **call stack** overhead in recursive algorithms. Every time a function calls itself recursively, the system allocates a stack frame to hold local variables, return addresses, and arguments.

### Example: Depth of Recursion
```cpp
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```
- Even though we aren't explicitly allocating arrays or lists, this function calls itself $n$ times. 
- The maximum depth of the recursion tree is $n$.
- Therefore, the auxiliary space complexity is **$O(n)$**.

### Example: Balanced Tree Recursion
```cpp
// Classic Merge Sort
void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}
```
- In Merge Sort, the recursion tree splits in half. The maximum depth of the call stack at any point in time is $\log_2(n)$.
- Thus, the recursion stack requires **$O(\log n)$** space.
- *(Note: Merge Sort also requires $O(n)$ auxiliary space for the `merge` arrays, making the total auxiliary space $O(n)$).*

> [!tip] 
> Tail Recursion Optimization: Some compilers can optimize tail-recursive functions (where the recursive call is the very last operation) to execute in $O(1)$ space by reusing the current stack frame. However, this is not guaranteed in all languages (e.g., Python does not support TCO).

---

## Time-Space Tradeoff
In algorithm design, you frequently encounter the **Time-Space Tradeoff**. You can often make an algorithm run faster by using more memory, or use less memory by spending more time computing.
- **Example**: Memoization in Dynamic Programming. We use an $O(n)$ array to cache previously computed values (increasing space), which drops the time complexity of naive Fibonacci from $O(2^n)$ down to $O(n)$.
