---
type: concept
tags: [hashing, cpp, strings, math]
date: 2026-06-30
---
# Fizz Buzz Mapping

## Problem Statement
Given a boundary integer $N$, construct an array of strings mapping sequentially from $1$ to $N$. If an index $i$ is mathematically divisible by $3$, append `"Fizz"`. If $i$ is mathematically divisible by $5$, append `"Buzz"`. If it satisfies both congruences, append `"FizzBuzz"`. Otherwise, append the string representation of $i$.

---

## Approach: Algorithmic Concatenation

Instead of defining overlapping mutually exclusive branches (`if i % 15 == 0`, `else if i % 3 == 0`, `else if i % 5 == 0`), we mathematically decouple the conditions.
By initializing an empty string and consecutively appending evaluations, the compound condition $i \equiv 0 \pmod{15}$ is structurally fulfilled by the sequential evaluation of $i \equiv 0 \pmod 3$ followed by $i \equiv 0 \pmod 5$.

While a [[Hash Map]] could theoretically bind arbitrary integers to strings, static `if` branching is strictly optimal for hardcoded constraints (3 and 5), reducing localized cache miss penalties.

---

## Code Implementation

```cpp
#include <vector>
#include <string>

using namespace std;

vector<string> fizzBuzz(int n) {
    vector<string> res;
    res.reserve(n); // Pre-allocate spatial bounds to avoid O(N) reallocation copies
    
    for (int i = 1; i <= n; i++) {
        string current = "";
        
        // Decoupled Modulo Congruence
        if (i % 3 == 0) current += "Fizz";
        if (i % 5 == 0) current += "Buzz";
        
        // Identity fallback mapping
        if (current.empty()) {
            current = to_string(i);
        }
        
        res.push_back(current);
    }
    
    return res;
}
```

> [!warning]
> Pre-allocating bounds via `res.reserve(n)` is an absolutely critical optimization for string collections. A dynamically expanding vector of `std::string` triggers heavily expensive reallocation, mathematically copying massive heap-allocated character arrays during the capacity scaling vector growth.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. The loop strictly evaluates $N$ scalars.
- **Space Complexity:** $\mathcal{O}(N)$ to store the structural bounds of the output sequence. Auxiliary space (excluding output arrays) is $\mathcal{O}(1)$.
