# Asymptotic Analysis

## What is Asymptotic Analysis?
- asymptotic Analysis is a mathematical framework used to describe the **limiting behavior** of an algorithm's performance (in terms of time or space) as the size of the input $n$ grows towards infinity.

- instead of counting exact CPU cycles or measuring milliseconds (which depend entirely on the specific machine, operating system, compiler, and background processes), asymptotic analysis evaluates how the resource consumption *scales*.

> [!tip] 
> The core question of asymptotic analysis is: **"If we double the input size, how much does the running time increase?"**


## Why Asymptotic Analysis?
- **machine Independence:** It provides a universal language to compare the theoretical efficiency of two algorithms, independent of hardware.
- **scalability Focus:** For very small inputs, almost any algorithm is fast. The true bottleneck appears when $n$ is massive. Asymptotic analysis specifically looks at the behavior for very large $n$.
- **simplicity:** By dropping lower-order terms and constants, the mathematical analysis becomes vastly simpler while retaining the most important property of the algorithm—its [[Order of Growth]].


## The Three Main Bounds
- to categorize the performance of an algorithm asymptotically, we use three primary mathematical notations:

- **[[big-O]] ($O$) - The Upper Bound**
   - describes the worst-case scenario.
   - guarantees that the algorithm will take *no more* than a certain amount of time.

- **[[big-Omega]] ($\Omega$) - The Lower Bound**
   - describes the best-case scenario or theoretical floor.
   - guarantees that the algorithm will take *at least* a certain amount of time.

- **[[theta]] ($\Theta$) - The Tight Bound**
   - occurs when the upper and lower bounds are identical.
   - gives the exact rate of growth.


## Example: Best, Worst, and Average Cases

- in asymptotic analysis, we evaluate an algorithm under different conditions:

```cpp
// Linear Search
int search(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

- **best-Case Analysis ($\Omega$):** The element is found at the very first index `arr[0]`. The loop runs only once. The best-case time complexity is $\Omega(1)$.
- **worst-Case Analysis ($O$):** The element is not in the array, or is at the very end. The loop runs $n$ times. The worst-case time complexity is $O(n)$.
- **average-Case Analysis:** The element is equally likely to be anywhere. On average, we search $n/2$ elements. We drop the constant factor ($1/2$), so the average-case complexity is asymptotically bounded by $O(n)$.

NEXT: [[Index]]
