---
type: concept
tags: [linked-list, cpp, design, priority-queue]
date: 2026-06-30
---
# Design Twitter

## Problem Statement
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and dynamically fetch the 10 most recent tweets directly in the user's news feed.

## Approach / Intuition
Maintain scalable maps tracking user followers and lists of their posts. When actively generating the news feed, gather all posts from the user and their direct followees. Use a max-heap to aggressively merge and retrieve the top 10 most recent tweets in timestamp order, merging [[Hash Map Tracking]] effectively with a [[Priority Queue]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(U) for follow/unfollow, O(T log 10) for getNewsFeed
- **[[Space Complexity]]:** O(U + T)

## Sample Code
```cpp
class Twitter {
    int time = 0;
    unordered_map<int, unordered_set<int>> followers;
    unordered_map<int, vector<pair<int, int>>> tweets;
public:
    Twitter() {}
    void postTweet(int userId, int tweetId) {
        tweets[userId].push_back({time++, tweetId});
    }
    vector<int> getNewsFeed(int userId) {
        priority_queue<pair<int, int>> pq;
        followers[userId].insert(userId);
        for (int followee : followers[userId]) {
            for (auto& tweet : tweets[followee]) {
                pq.push(tweet);
            }
        }
        vector<int> res;
        while (!pq.empty() && res.size() < 10) {
            res.push_back(pq.top().second);
            pq.pop();
        }
        return res;
    }
    void follow(int followerId, int followeeId) {
        followers[followerId].insert(followeeId);
    }
    void unfollow(int followerId, int followeeId) {
        followers[followerId].erase(followeeId);
    }
};
```

## New Keywords / STL Used
`std::unordered_set`

## Edge Cases
Users explicitly attempting to follow/unfollow themselves, requesting feeds for users completely lacking social ties or posts.
