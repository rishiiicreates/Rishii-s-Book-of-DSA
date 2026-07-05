# Word Ladder II

## Problem Statement
- given `beginWord`, `endWord`, and a `wordList`, mathematically compute and return **all absolute shortest transformation sequences** from `beginWord` to `endWord`.

- unlike [[Word Ladder I]], returning merely the integer depth is insufficient; you must extract the exhaustive structural paths.


## Approach: BFS to Build DAG + DFS to Backtrack Paths

- extracting all shortest paths requires a highly engineered dual-phase algorithm. A standard BFS storing entire vectors in the queue will trigger devastating memory limit exceed (MLE) errors due to combinatorial explosion.

- **phase 1: Level-Synchronized BFS**
   - we must discover the shortest paths while simultaneously tracking the topological parent-child relationships.
   - we maintain a `unordered_map<string, unordered_set<string>> parentMap` to store the Directed Acyclic Graph (DAG) of the shortest paths backwards (from child to parents).
   - because multiple parents on the *same depth level* can mathematically map to the identical child, we cannot erase a word from the global `wordSet` the instant we generate it. We must erase all visited words *only at the end of the respective BFS level*.
- **phase 2: Depth-First Search Backtracking**
   - beginning rigidly at `endWord`, we execute a recursive DFS utilizing the `parentMap` to traverse backward to `beginWord`.
   - every complete path generated is reversed (as it was built backwards) and appended to the global result [[Matrix]].


## Code Implementation (Optimized DAG Construction)

```cpp
#include <string>
#include <vector>
#include <unordered_set>
#include <unordered_map>
#include <queue>
#include <algorithm>
using namespace std;

// Phase 2: Recursive Backtracking
void dfs(string word, string& beginWord, unordered_map<string, unordered_set<string>>& parentMap, 
         vector<string>& path, vector<vector<string>>& result) {
    if (word == beginWord) {
        vector<string> validPath = path;
        reverse(validPath.begin(), validPath.end());
        result.push_back(validPath);
        return;
    }
    for (const string& p : parentMap[word]) {
        path.push_back(p);
        dfs(p, beginWord, parentMap, path, result);
        path.pop_back();
    }
}

// Phase 1: BFS DAG Construction
vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
    unordered_set<string> wordSet(wordList.begin(), wordList.end());
    vector<vector<string>> result;
    if (wordSet.find(endWord) == wordSet.end()) return result;

    unordered_map<string, unordered_set<string>> parentMap;
    unordered_set<string> currentLevel, nextLevel;
    
    currentLevel.insert(beginWord);
    bool found = false;

    while (!currentLevel.empty() && !found) {
        for (const string& word : currentLevel) {
            wordSet.erase(word); // Level synchronization block
        }

        for (const string& word : currentLevel) {
            string mutated = word;
            for (int i = 0; i < mutated.length(); ++i) {
                char original = mutated[i];
                for (char ch = 'a'; ch <= 'z'; ++ch) {
                    mutated[i] = ch;
                    if (wordSet.find(mutated) != wordSet.end()) {
                        nextLevel.insert(mutated);
                        parentMap[mutated].insert(word); // Reverse mapping
                        if (mutated == endWord) found = true;
                    }
                }
                mutated[i] = original;
            }
        }
        currentLevel = nextLevel;
        nextLevel.clear();
    }

    if (found) {
        vector<string> path = {endWord};
        dfs(endWord, beginWord, parentMap, path, result);
    }
    return result;
}
```

> [!warning]
> The level-synchronized memory management (`wordSet.erase` bounded outside the mutation loop) is the absolute linchpin of this algorithm. Without it, you artificially prune valid shortest paths deriving from different parents on the identical topological tier.


## Complexity Analysis
- **time Complexity:** $O(N \times L \times 26 + P \times L)$, where $P$ is the total quantity of shortest paths. The combinatorial bound $P$ can mathematically scale exponentially in highly dense graphs (e.g., $O(2^N)$), making the worst-case bound inherently exponential.
- **space Complexity:** $O(N \times L)$ to store the `parentMap` DAG and dictionaries, plus $O(P \times L)$ to explicitly store the output paths.

NEXT: [[Index]]
