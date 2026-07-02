---
type: concept
tags: [array, string, cpp, math, simulation]
date: 2026-06-30
---
# Multiply Large Numbers

## Problem Statement
Given two non-negative integers represented as strings `num1` and `num2`, return the product of `num1` and `num2`, also represented as a string.
*Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.*

---

## Approach: Mathematical Convolution Simulation

When multiplying two numbers of lengths $N$ and $M$, the maximum possible length of their product is strictly bounded by $N + M$. 
We simulate the traditional manual elementary school multiplication process.

Instead of computing row sums separately and adding them later (which requires complex dynamic memory and string addition), we directly accumulate the partial products into a fixed-size integer array of length $N + M$.
For any digit `num1[i]` and `num2[j]`:
1. Their scalar product affects the resulting number at indices `i + j` and `i + j + 1`.
2. The index `i + j + 1` holds the unit place of the scalar product.
3. The index `i + j` holds the carry-over.

By iterating backwards (from least significant digit to most significant digit) for both strings, we can continuously accumulate the products and propagate the carry systematically.

---

## Code Implementation

```cpp
#include <string>
#include <vector>
using namespace std;

string multiply(string num1, string num2) {
    // Edge case: multiplication by zero
    if (num1 == "0" || num2 == "0") return "0";
    
    int n = num1.length();
    int m = num2.length();
    
    // Result array can be at most size n + m
    vector<int> res(n + m, 0);
    
    // Traverse from right to left
    for (int i = n - 1; i >= 0; i--) {
        for (int j = m - 1; j >= 0; j--) {
            int mul = (num1[i] - '0') * (num2[j] - '0');
            
            // Add the product to the existing value at the current position
            int sum = mul + res[i + j + 1];
            
            // Store the unit digit at i + j + 1
            res[i + j + 1] = sum % 10;
            
            // Add the carry to i + j
            res[i + j] += sum / 10;
        }
    }
    
    // Skip leading zeros and convert back to string
    string ans = "";
    int i = 0;
    while (i < res.size() && res[i] == 0) {
        i++;
    }
    
    while (i < res.size()) {
        ans += to_string(res[i]);
        i++;
    }
    
    return ans;
}
```

> [!tip]
> The relationship between indices `(i, j)` and the result array positions `(i + j, i + j + 1)` is the core mathematical realization that transforms a messy string manipulation problem into an elegant array mapping.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \times M)$, where $N$ and $M$ are the lengths of `num1` and `num2`. We iterate through every digit combination exactly once.
- **Space Complexity:** $\mathcal{O}(N + M)$ to store the intermediate integer representation of the product before string conversion.
