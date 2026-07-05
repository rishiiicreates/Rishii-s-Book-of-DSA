# Hashing: The Core Theory

## Problem Statement
- design a data structure that maps an abstract key $K$ to a specific memory location, enabling $O(1)$ expected time complexity for insertion, retrieval, and deletion operations.


## Approach: Mathematical Mapping (Hash Functions)

- hashing is a mathematical technique that maps a large, sparse universe of possible keys $U$ into a smaller, dense universe of indices $V$ using a Hash Function $H: U \to V$. This forms the foundation of the [[Hash Map]] and [[Hash Set]].

- **the Hash Function:** $h(K) = \text{index}$. A good hash function is deterministic, fast to compute, and uniformly distributes keys across the available indices to minimize collisions.
   - a common universal hashing technique for integers is $h(x) = ((a \cdot x + b) \bmod p) \bmod M$, where $p$ is a large prime and $M$ is the table size.
- **the Hash Table:** An array of size $M$. The element with key $K$ is stored at index $h(K)$.
- **collisions:** Because $|U| \gg |V|$, by the Pigeonhole Principle, there will exist $K_1 \neq K_2$ such that $h(K_1) = h(K_2)$. This is a Hash Collision. We must employ collision resolution strategies like [[Separate Chaining]], [[Linear Probing]], or [[Quadratic Probing]].


## Code Implementation (Basic Hash Table with Modulo)

```cpp
#include <vector>
#include <list>

using namespace std;

class MyHash {
private:
    int capacity;
    // Using Separate Chaining for this abstract model
    vector<list<int>> table;
    
    // Simple modulo hash function
    int hashFunction(int key) const {
        // Handle negative keys gracefully in C++
        return (key % capacity + capacity) % capacity;
    }

public:
    MyHash(int cap) : capacity(cap), table(cap) {}
    
    void insert(int key) {
        int index = hashFunction(key);
        // Avoid duplicates for a Set-like behavior
        for (int k : table[index]) {
            if (k == key) return; 
        }
        table[index].push_back(key);
    }
    
    bool search(int key) const {
        int index = hashFunction(key);
        for (int k : table[index]) {
            if (k == key) return true;
        }
        return false;
    }
    
    void remove(int key) {
        int index = hashFunction(key);
        table[index].remove(key);
    }
};
```

> [!important]
> The load factor $\alpha = \frac{N}{M}$ (where $N$ is the number of elements and $M$ is the capacity) dictates performance. When $\alpha$ exceeds a threshold (e.g., $0.75$), the table must be dynamically resized and completely rehashed to maintain $O(1)$ operations.


## Complexity Analysis
- **time Complexity:** $O(1)$ expected for insert, search, and delete assuming Uniform Hashing. $O(N)$ worst-case if all elements collide to the same hash value.
- **space Complexity:** $O(N + M)$ where $N$ is the number of elements and $M$ is the number of buckets.

NEXT: [[Index]]
