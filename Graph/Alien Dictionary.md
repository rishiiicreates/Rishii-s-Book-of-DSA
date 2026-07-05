# Alien Dictionary

## Problem Statement
- given a topological array of string words geometrically sorted in a generic **Alien Language Lexicographical Order**. The alphabet consists of explicit lowercase characters. Mathematically compute a string validating a structurally sound topological sorted order of explicit characters. If multiple spatial orders map valid geometries, output any one sequence. If topological contradictions exist (cycles) or if structural prefix anomalies arise (e.g., `"abcd"` pre-dating `"abc"`), output an empty sequence `""`.


## Approach: Character Directed Graph & Kahn's Topological Sort

- the lexicographical ordering algorithm isolates strict character dependency edges $A \to B$ when comparing contiguous topological strings.
- **geometric Anomaly Check:** If `word1` strictly dominates `word2` visually (e.g., `word1 = "abcd"`, `word2 = "abc"`), the prefix logic crashes inherently. Return `""`.
- **topological Graph Extraction:** Iterate every contiguous pair of words (`dict[i]` and `dict[i+1]`). Iteratively scan matching geometric characters. The absolute first topological scalar mismatch maps a definitive directed edge: `char_1 -> char_2`.
- **kahn's Topological Extraction:**
   - execute an Adjacency [[Matrix]] aggregating explicitly bound characters.
   - map algebraic `in-degree` limits for characters.
   - push independent character bounds (`in-degree == 0`) to a BFS sequence queue.
   - if the sequence exhausts completely containing all $K$ valid explicit alphabet characters, append to result. If cyclic contradictions force premature exhaustion, return `""`.


## Code Implementation

```cpp
#include <vector>
#include <string>
#include <queue>

using namespace std;

string findOrder(string dict[], int N, int K) {
    // Construct Topological Geometry
    vector<vector<int>> adj(K);
    vector<int> indegree(K, 0);
    
    // Map consecutive string anomalies and extract structural edges
    for (int i = 0; i < N - 1; i++) {
        string s1 = dict[i];
        string s2 = dict[i+1];
        
        int len = min(s1.size(), s2.size());
        bool mismatch_found = false;
        
        for (int j = 0; j < len; j++) {
            if (s1[j] != s2[j]) {
                // Lexicographical edge maps definitively: s1[j] -> s2[j]
                adj[s1[j] - 'a'].push_back(s2[j] - 'a');
                indegree[s2[j] - 'a']++;
                mismatch_found = true;
                break; // Mismatch structurally anchors dependency
            }
        }
        
        // Impossible Geometry constraint: s1 is longer but fully encapsulates s2 sequence
        if (!mismatch_found && s1.size() > s2.size()) {
            return "";
        }
    }
    
    // Execute Kahn's Toplogy Sequence
    queue<int> q;
    for (int i = 0; i < K; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    string topo = "";
    while (!q.empty()) {
        int curr = q.front();
        q.pop();
        
        topo += (char)(curr + 'a');
        
        for (auto next_char : adj[curr]) {
            indegree[next_char]--;
            if (indegree[next_char] == 0) {
                q.push(next_char);
            }
        }
    }
    
    // Deadlock detection cycle constraint
    if (topo.size() == K) return topo;
    
    return "";
}
```


## Complexity Analysis
- **time Complexity:** $O(N \cdot L + K)$ geometric bound limit, where $N$ maps the size of the array, $L$ maps scalar string length constraint, and $K$ isolates explicit topological alphabets. Character extraction evaluates linear to sequence geometry.
- **space Complexity:** $O(K + E)$ bounding explicit adjacency array coordinates. $K$ limits vertices, $E$ bounded edges constraint limits to $N$.

> [!important]
> **Edge Redundancy Over-Saturation:** Consecutive string scanning natively spawns redundant topological edges ($A \to B$ processed sequentially). The standard Adjacency vector `vector<int> adj[]` safely maps redundant geometric overlaps, inflating $in-degree$ symmetrically without destroying Kahn's validation mechanism.

NEXT: [[Index]]
