---
type: concept
tags: [graph, cpp, backtracking]
date: 2026-06-30
---
# Remove Invalid Parentheses

## Problem Statement
Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible valid strings.

## Approach / Intuition
Use [[BFS]] or [[Backtracking]] to generate all possible states by removing one parenthesis at a time. The first level in BFS that yields valid strings guarantees the minimum removals.

## Time & Space Complexity
- **[[Time Complexity]]:** O(2^N)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <string>
#include <unordered_set>
#include <queue>
using namespace std;

bool isValid(string s) {
    int count = 0;
    for(char c : s) {
        if(c == '(') count++;
        if(c == ')') count--;
        if(count < 0) return false;
    }
    return count == 0;
}

vector<string> removeInvalidParentheses(string s) {
    vector<string> res;
    unordered_set<string> visited;
    queue<string> q;
    q.push(s);
    visited.insert(s);
    bool found = false;
    while(!q.empty()) {
        string curr = q.front();
        q.pop();
        if(isValid(curr)) {
            res.push_back(curr);
            found = true;
        }
        if(found) continue;
        for(int i = 0; i < curr.length(); i++) {
            if(curr[i] != '(' && curr[i] != ')') continue;
            string next = curr.substr(0, i) + curr.substr(i + 1);
            if(visited.find(next) == visited.end()) {
                q.push(next);
                visited.insert(next);
            }
        }
    }
    return res;
}
```

## New Keywords / STL Used
`std::queue`, `std::unordered_set`

## Edge Cases
Already valid string, string without parentheses, no valid string possible.
