# Analysis of Recursion

## How to Analyze Recursion
- analyzing loops is straightforward: you multiply the inner operations by the loop count. But analyzing recursion requires solving mathematical equations or visualizing tree structures, because the function calls branch out dynamically.

- we evaluate the efficiency of a recursive function using two primary metrics: **[[Time Complexity]]** and **[[Space Complexity]]**.


## 1. Space Complexity Analysis

- the space complexity of a recursive algorithm is **never $O(1)$** unless the compiler uses Tail Call Optimization.

- every recursive call allocates a new frame on the **Call Stack** containing the local variables and return address. Therefore, the space complexity is exactly proportional to the **Maximum Depth of the Recursion Tree** at any given moment.

- **linear Recursion (e.g., Factorial):** The depth is $N$. Space is $O(N)$.
- **divide and Conquer (e.g., Merge Sort):** The problem is halved each time. The depth is $\log N$. Space is $O(\log N)$.

> [!important]
> Even if a function spawns two branches at each step (like naive Fibonacci calculating $2^N$ nodes), the space complexity is only the *depth* of the tree ($O(N)$), because it fully traverses down one branch and pops it off the stack before going down the other.


## 2. Time Complexity Analysis

- to find the time complexity, we formulate a **Recurrence Relation**—a mathematical equation that defines the sequence recursively.

### Method 1: The Recursion Tree Method
- we draw a tree of all function calls, compute the work done at each node, and sum the work level by level.

- **example: Merge Sort**
$$ T(n) = 2T(n/2) + O(n) $$
- **level 0:** 1 node doing $n$ work.
- **level 1:** 2 nodes doing $n/2$ work each $\rightarrow 2 \times (n/2) = n$ work.
- **level 2:** 4 nodes doing $n/4$ work each $\rightarrow 4 \times (n/4) = n$ work.
- **total Levels:** The tree halves until it reaches 1, so there are $\log_2(n)$ levels.
- **total Time:** $n \text{ work/level} \times \log_2(n) \text{ levels} = \Theta(n \log n)$.

### Method 2: The Master Theorem
- for recurrence relations of the form:
$$T(n) = aT(n/b) + f(n)$$
- where $a \ge 1$ (branching factor), $b > 1$ (subproblem shrink factor), and $f(n)$ is the cost of the work done outside the recursive calls.

- let $c = \log_b a$.
- if $f(n) = O(n^{c-\epsilon})$, then $T(n) = \Theta(n^c)$.
- if $f(n) = \Theta(n^c)$, then $T(n) = \Theta(n^c \log n)$.
- if $f(n) = \Omega(n^{c+\epsilon})$, then $T(n) = \Theta(f(n))$.

- *for a full breakdown of bounds, refer back to the core [[Time Complexity]] notes.*

NEXT: [[Index]]
