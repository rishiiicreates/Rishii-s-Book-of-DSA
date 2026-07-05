# Lexicographic Rank

## Problem Statement
- given a string $S$ of unique characters, mathematically compute its lexicographic rank among all possible permutations of its characters.
- *example:* `S = "CBA"`. The permutations are `"ABC"`, `"ACB"`, `"BAC"`, `"BCA"`, `"CAB"`, `"CBA"`. The rank of `"CBA"` is 6.


## Approach: Combinatorial Counting

- to find the rank of a string without generating all permutations, we calculate exactly how many valid permutations come *before* it in lexicographical order.

- for a string of length $N$, the total number of permutations is $N!$.
- at any index $i$, if we fix a character smaller than $S[i]$, the remaining $(N - 1 - i)$ characters can be arranged in $(N - 1 - i)!$ ways.

- the algorithm proceeds as follows:
- traverse the string from left to right ($i$ from $0$ to $N-1$).
- for the character $S[i]$, count how many characters present in the remaining suffix $S[i+1 \dots N-1]$ are strictly smaller than $S[i]$. Let this count be $C$.
- these $C$ smaller characters could have been placed at index $i$. For each such placement, there are $(N - 1 - i)!$ valid permutations.
- add $C \times (N - 1 - i)!$ to the total rank count.
- after scanning the whole string, add $1$ (since the calculated sum is the number of permutations strictly *before* $S$, and we want the 1-based rank).

- to do this efficiently, we can use a frequency array or Binary Indexed Tree (Fenwick Tree) to dynamically query the count of smaller available characters in $O(1)$ or $O(\log \Sigma)$ time.


## Code Implementation

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

// Helper function to precompute factorials
long long fact(int n) {
    long long f = 1;
    for (int i = 2; i <= n; i++) f *= i;
    return f;
}

long long lexicographicRank(string s) {
    int n = s.length();
    long long rank = 1; // 1-based indexing
    long long mul = fact(n); // N!
    
    // Frequency array for character presence
    vector<int> count(256, 0);
    for (char c : s) {
        count[c]++;
    }
    
    // Convert frequency array into a prefix sum array
    // count[i] now stores how many available characters are <= i
    for (int i = 1; i < 256; i++) {
        count[i] += count[i - 1];
    }
    
    for (int i = 0; i < n; i++) {
        mul /= (n - i); // Update the factorial multiplier to (N - 1 - i)!
        
        // Count of characters strictly smaller than s[i]
        int smaller_chars = count[s[i] - 1];
        
        rank += smaller_chars * mul;
        
        // Remove s[i] from the available characters pool
        // This means decrementing the count for all ASCII values >= s[i]
        for (int j = s[i]; j < 256; j++) {
            count[j]--;
        }
    }
    
    return rank;
}
```


## Complexity Analysis
- **time Complexity:** $O(N \times \Sigma)$ where $N$ is the length of the string and $\Sigma$ is the alphabet size ($256$ for ASCII). Updating the prefix array takes $O(\Sigma)$, making the overall time $O(N)$.
- **space Complexity:** $O(\Sigma)$ for the character counting array.

> [!warning]
> **Duplicate Characters:** The formula drastically changes if the string contains duplicate characters. Instead of simply $(N-1-i)!$, you must divide by the product of the factorials of the frequencies of all remaining characters.

NEXT: [[Index]]
