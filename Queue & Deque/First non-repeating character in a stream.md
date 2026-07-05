# First non-repeating character in a stream

## Problem Statement
- given a stream of characters, find the first non-repeating character every time a new character is added.

## Approach / Intuition
- use a frequency array (or map) to track character counts and a [[Queue]] to maintain the order of characters. When a new character arrives, update its frequency and push it to the queue. Then, pop characters from the front of the queue if their frequency is greater than 1, until a non-repeating character is found or the queue is empty.

## Time & Space Complexity
- **[[time Complexity]]:** O(1) amortized per character
- **[[space Complexity]]:** O(n) or O(1) considering fixed character set size

## Sample Code
```cpp
string firstNonRepeating(string A) {
    vector<int> freq(26, 0);
    queue<char> q;
    string res = "";
    for (char c : A) {
        freq[c - 'a']++;
        q.push(c);
        while (!q.empty() && freq[q.front() - 'a'] > 1) {
            q.pop();
        }
        if (q.empty()) {
            res += '#';
        } else {
            res += q.front();
        }
    }
    return res;
}
```

## New Keywords / STL Used
- `queue<char>`

## Edge Cases
- all characters repeat, no characters repeat, long stream with late repeats.

NEXT: [[Index]]
