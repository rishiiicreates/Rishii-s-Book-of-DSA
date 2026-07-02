---
type: concept
tags: [heap, design, cpp, max-heap, oops]
date: 2026-07-01
---
# Design Twitter

## Problem Statement
Mathematically architect a scalable, simplified topological network of a social media system. The architecture must geometrically isolate and support:
1. `postTweet(userId, tweetId)`: Mutate sequence state.
2. `getNewsFeed(userId)`: Extract the 10 most temporally recent tweets propagated by the user and their explicit geometric followers, sorted sequentially descending.
3. `follow(followerId, followeeId)`: Architect a directed topological edge.
4. `unfollow(followerId, followeeId)`: Sever a directed topological edge.

---

## Approach: Directed Graphs & Max-Heap K-Way Merge

This is an architectural composition of two primary data models:
1. **Geometric Relational Map (Adjacency List):** `unordered_map<int, unordered_set<int>>` mapping absolute explicit follow topologies.
2. **Temporal State Map:** `unordered_map<int, vector<pair<int, int>>>` binding users to their ordered discrete tweet sequences (timestamp, tweetId).

The `getNewsFeed` algorithm structurally demands extracting the 10 most recent chronological scalars from up to $K$ distinct sorted lists (the user's topological followings).
This is a strict mathematical equivalent to **Merging $K$ Sorted Linked Lists**.
We inject a globally monotonic `timestamp` integer. We isolate the absolute tail (most recent) of every valid user sequence and push it to a **Max-Heap**.
Iteratively extract the mathematical maximum temporal scalar, push it to the output bounds, and conceptually advance the sequence pointer for that specific sub-array, injecting the next temporal element until 10 scalars are isolated.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>

using namespace std;

class Twitter {
private:
    int global_time; // Absolute Monotonic Temporal Sequence
    
    // Relational Graph: follower -> set of followees
    unordered_map<int, unordered_set<int>> friends;
    
    // Temporal Sequence Map: userId -> vector<{time, tweetId}>
    unordered_map<int, vector<pair<int, int>>> tweets;

public:
    Twitter() {
        global_time = 0;
    }
    
    void postTweet(int userId, int tweetId) {
        tweets[userId].push_back({global_time++, tweetId});
    }
    
    vector<int> getNewsFeed(int userId) {
        // Max-Heap: {time, tweetId, userId, index_in_user_vector}
        priority_queue<vector<int>> pq;
        
        // Extract geometry for explicit user
        if (!tweets[userId].empty()) {
            int last_idx = tweets[userId].size() - 1;
            pq.push({tweets[userId][last_idx].first, tweets[userId][last_idx].second, userId, last_idx});
        }
        
        // Extract geometry for topological followees
        for (int followeeId : friends[userId]) {
            if (!tweets[followeeId].empty()) {
                int last_idx = tweets[followeeId].size() - 1;
                pq.push({tweets[followeeId][last_idx].first, tweets[followeeId][last_idx].second, followeeId, last_idx});
            }
        }
        
        vector<int> feed;
        // Strict Sequence K-Way Merge Algorithm (Max bounds = 10)
        while (!pq.empty() && feed.size() < 10) {
            auto top = pq.top();
            pq.pop();
            
            feed.push_back(top[1]);
            
            int uId = top[2];
            int prev_idx = top[3] - 1;
            
            // Propagate sub-sequence spatial boundary
            if (prev_idx >= 0) {
                pq.push({tweets[uId][prev_idx].first, tweets[uId][prev_idx].second, uId, prev_idx});
            }
        }
        
        return feed;
    }
    
    void follow(int followerId, int followeeId) {
        if (followerId != followeeId) {
            friends[followerId].insert(followeeId);
        }
    }
    
    void unfollow(int followerId, int followeeId) {
        friends[followerId].erase(followeeId);
    }
};
```

---

## Complexity Analysis
- **`postTweet` / `follow` / `unfollow`:** $O(1)$ amortized temporal bounds leveraging raw `unordered_map` and `unordered_set` hash constraints.
- **`getNewsFeed`:** $O(F + 10 \log F)$ time. Constructing the initial max-heap operates in $O(F \log F)$ or $O(F)$ heapify, where $F$ is the geometrical cardinality of explicit followings. Iterative extractions structurally bound to a constant 10 $\log$ bounds.
- **Space Complexity:** $O(U + T)$ absolute limit mapping Total Users and Total Tweets geometrically mapped into dynamic memory arrays.
