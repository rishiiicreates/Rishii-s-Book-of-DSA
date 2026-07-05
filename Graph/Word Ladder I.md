# Word Ladder I

## Problem Statement
- given two strings `beginWord` and `endWord`, and a dictionary of strings `wordList`, mathematically compute the absolute shortest transformation sequence length from `beginWord` to `endWord`.

- a strictly valid transformation sequence must satisfy:
- every adjacent pair of words $W_i$ and $W_{i+1}$ differ by exactly one character.
- every intermediate word $W_i$ must exist within the `wordList`.

- if no such transformation sequence exists, return `0`.


## Approach: Unweighted Graph Shortest Path via BFS

- this problem fundamentally maps to determining the **Shortest Path in an Unweighted Graph**.
- each unique word constitutes a **Vertex** $V$.
- an **Edge** exists between two vertices if their Hamming distance is precisely $1$.

- because the edges are strictly unweighted (or uniformly weighted $W=1$), [[Breadth First Search]] inherently guarantees finding the absolute shortest path. DFS is mathematically incapable of guaranteeing shortest paths in unweighted graphs without exhaustive exponential exploration.

- **topological Mapping:** Instead of precomputing the $O(V^2)$ adjacency list (which is catastrophic for large dictionaries), we dynamically generate neighbors. For a given word, iteratively replace each character with every letter $a-z$ and check if the resulting mutation exists in a `unordered_set` derived from `wordList`.
- **state Tracking:**
   - instantiate a queue holding pairs `{current_word, current_depth}`.
   - delete words from the `unordered_set` immediately upon visitation to enforce strict acyclic progression.
- **termination:** If the generated mutation equals `endWord`, immediately return `current_depth + 1`.


## Code Implementation

```cpp
#include <string>
#include <vector>
#include <unordered_set>
#include <queue>
using namespace std;

int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    // $O(1)$ lookup state dictionary
    unordered_set<string> wordSet(wordList.begin(), wordList.end());
    
    // Mathematical boundary constraint
    if (wordSet.find(endWord) == wordSet.end()) return 0;
    
    queue<pair<string, int>> q;
    q.push({beginWord, 1});
    wordSet.erase(beginWord); // Lock topological node to prevent cycles
    
    while (!q.empty()) {
        string word = q.front().first;
        int depth = q.front().second;
        q.pop();
        
        // Terminal node reached
        if (word == endWord) return depth;
        
        // Dynamic edge enumeration
        for (int i = 0; i < word.length(); ++i) {
            char original = word[i];
            
            for (char ch = 'a'; ch <= 'z'; ++ch) {
                word[i] = ch;
                
                // Discovered valid connected component
                if (wordSet.find(word) != wordSet.end()) {
                    wordSet.erase(word); // Enforce acyclic constraint
                    q.push({word, depth + 1});
                }
            }
            // Backtrack state for subsequent permutations
            word[i] = original;
        }
    }
    
    return 0; // Graph disconnected relative to target
}
```

> [!important]
> Notice the crucial `wordSet.erase(word)` statement. This acts as the explicit `visited` boolean array. Without it, the BFS will oscillate infinitely between `hit -> hot -> hit`, shattering the memory limit.


## Complexity Analysis
- **time Complexity:** $O(N \times L \times 26) \approx O(N \times L)$, where $N$ is the scalar length of the `wordList` and $L$ is the length of the string. In the absolute worst case, every single word is processed, and for each word, $26 \times L$ dynamic mutations are hashed.
- **space Complexity:** $O(N \times L)$ mapping to the `unordered_set` overhead to store all dictionary words, plus the theoretical maximum queue depth.

NEXT: [[Index]]
