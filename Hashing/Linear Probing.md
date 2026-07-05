# Linear Probing (Open Addressing)

## Problem Statement
- implement a collision resolution strategy for a hash table that stores all elements directly within the array (Open Addressing), without using external linked lists.


## Approach: Sequential Searching

- linear Probing is the simplest form of [[Open Addressing]]. When a hash collision occurs at index $h(K)$, we linearly check the next slots: $h(K)+1$, $h(K)+2$, $\dots$ (wrapping around the end of the array) until an empty slot is found.

- **probe Sequence:** The $i$-th probe for key $K$ is determined by the mathematical function:
   $$ P(K, i) = (h(K) + i) \bmod M $$
   - where $i \in \{0, 1, 2, \dots, M-1\}$ and $M$ is the table capacity.
- **insertion:** Start at $P(K, 0)$. If occupied, probe $P(K, 1)$, then $P(K, 2)$, etc., until an empty slot is found.
- **search:** Follow the same probe sequence. Stop when you find the key (success) or reach an empty slot (failure).
- **deletion (Tombstones):** We cannot simply set a deleted slot to "empty", as this would break the search chain for elements inserted after it that collided at the same original index. We must mark the slot with a special "Tombstone" (e.g., a dummy value like $-2$) to indicate it is deleted but the search should continue past it.


## Code Implementation

```cpp
#include <vector>
using namespace std;

class LinearProbingHash {
private:
    vector<int> table;
    int capacity;
    const int EMPTY = -1;
    const int DELETED = -2;

    int hash(int key) {
        return key % capacity;
    }

public:
    LinearProbingHash(int cap) : capacity(cap), table(cap, EMPTY) {}

    void insert(int key) {
        int index = hash(key);
        int i = 0;
        
        // Find empty slot or a deleted slot we can reuse
        while (table[(index + i) % capacity] != EMPTY && 
               table[(index + i) % capacity] != DELETED) {
            
            // If key already exists, don't insert duplicate
            if (table[(index + i) % capacity] == key) return;
            
            i++;
            if (i == capacity) return; // Table is completely full
        }
        
        table[(index + i) % capacity] = key;
    }

    bool search(int key) {
        int index = hash(key);
        int i = 0;
        
        while (table[(index + i) % capacity] != EMPTY) {
            if (table[(index + i) % capacity] == key) return true;
            i++;
            if (i == capacity) break; // Traversed whole table
        }
        return false;
    }
};
```

> [!warning]
> **Primary Clustering:** Linear probing suffers severely from primary clustering. As clusters of occupied slots form, the probability of the next insertion hitting that cluster and expanding it grows proportionally to the cluster size, rapidly degrading performance from $O(1)$ to $O(N)$.


## Complexity Analysis
- **time Complexity:** $O(1)$ expected for sparse tables ($\alpha \le 0.5$). Degrades to $O(N)$ worst-case due to clustering or full tables.
- **space Complexity:** $O(M)$ where $M$ is the table capacity.

NEXT: [[Index]]
