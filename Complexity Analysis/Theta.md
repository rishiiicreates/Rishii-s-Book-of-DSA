# Theta Notation ($\Theta$)

## Mathematical Definition
- theta notation describes the **asymptotic tight bound** of an algorithm. If an algorithm is bounded both from above and from below by the same function (up to constant factors), we use Theta notation.

- we say that $f(n) = \Theta(g(n))$ if there exist positive constants $c_1, c_2$, and $n_0$ such that:
$$ 0 \le c_1 \cdot g(n) \le f(n) \le c_2 \cdot g(n) \quad \text{for all } n \ge n_0 $$

### Intuition
- this means $f(n)$ is simultaneously $O(g(n))$ and $\Omega(g(n))$. The algorithm's true growth rate perfectly matches $g(n)$. It acts as an "equals" ($=$) relationship for growth rates.

> [!important]
> When someone in an interview asks for the "Big-O", they are almost always actually asking for the **Theta** bound. They want the *exact* rate of growth. Saying an $O(n)$ algorithm is $O(n^3)$ is mathematically true for Big-O, but false for Theta (and bad for interviews).


## When Can We Use Theta?

- you can only declare a tight bound $\Theta$ if the best-case and worst-case complexities have the exact same [[Order of Growth]].

### Example 1: Valid Theta (Merge Sort)
- merge Sort always divides the array in half and always merges all elements.
- best Case: $\Omega(n \log n)$
- worst Case: $O(n \log n)$
- therefore, Merge Sort's time complexity is strictly **$\Theta(n \log n)$**.

### Example 2: Invalid Theta (Insertion Sort)
- insertion Sort behaves differently depending on the input data.
- best Case (already sorted): $\Omega(n)$
- worst Case (reverse sorted): $O(n^2)$
- because $\Omega(n) \neq O(n^2)$, **there is no single $\Theta$ bound** that describes Insertion Sort overall. You must specify the case.

### Example 3: Always Linear
```cpp
void printArray(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        cout << arr[i] << "\n";
    }
}
```
- regardless of the array's contents, this function will execute exactly $n$ times. The runtime is perfectly bound by the length of the array, making it **$\Theta(n)$**.

NEXT: [[Index]]
