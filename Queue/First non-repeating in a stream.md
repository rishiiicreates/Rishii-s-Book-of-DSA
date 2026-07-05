# First Non-Repeating Character in a Stream

## Problem Statement
- given an continuous data stream of characters, mathematically identify the first non-repeating character observed so far at each discrete instance of time. If no non-repeating character currently exists, output `#` or $-1$.

- *example:* Stream = `a a b c`
- *result Array:* `[a, #, b, b]`


## Approach: Frequency Map + Queue

- tracking the first non-repeating character fundamentally mandates ordered preservation of character arrival and a constant-time mechanism to verify cardinality (frequency). We hybridize a [[Hash Map]] with a [[Queue]] to satisfy both constraints.

- **state Preservation:** Maintain an array/hash map `freq` to trace the absolute occurrences of every character.
- **order Preservation:** Maintain a queue $Q$ to store characters strictly in the order they emerge in the stream.
- **execution Loop:** As a character $C$ arrives:
   - increment its frequency: `freq[C]++`.
   - enqueue $C$ into $Q$.
   - **lazy Deletion Phase:** Evaluate the front of the queue. If `freq[Q.front()] > 1`, this element violates the uniqueness constraint. Pop it. Repeat this evaluation mathematically until the queue is empty or the front element is verified as unique (`freq[Q.front()] == 1`).
   - if $Q$ is empty after evaluation, return `#`. Otherwise, return `Q.front()`.

> [!important]
> We purposefully utilize **Lazy Deletion**. Instead of linearly scanning the queue to manually eject an element the exact moment its frequency hits 2, we leave it in the queue and naturally pop it only when it reaches the front. This optimization secures strict amortized $O(1)$ time complexity per insertion.


## Code Implementation

```cpp
#include <string>
#include <queue>
#include <vector>
using namespace std;

string FirstNonRepeating(const string& A) {
    // Assuming lowercase English alphabet space
    vector<int> freq(26, 0);
    queue<char> q;
    string result = "";
    
    for (char c : A) {
        // Increment frequency state
        freq[c - 'a']++;
        
        // Enqueue the candidate
        q.push(c);
        
        // Lazy deletion: purge violating elements from the front
        while (!q.empty() && freq[q.front() - 'a'] > 1) {
            q.pop();
        }
        
        // Determine system state
        if (q.empty()) {
            result += '#';
        } else {
            result += q.front();
        }
    }
    
    return result;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$, where $N$ is the number of characters processed. Every character is enqueued strictly once and dequeued at most once. Hence, the `while` loop yields an amortized constant $O(1)$ time per character.
- **space Complexity:** $O(|\Sigma|)$ where $\Sigma$ is the alphabet space (e.g., $O(26) = O(1)$ for lowercase characters). The queue will also never mathematically exceed $O(|\Sigma|)$ unique characters simultaneously before purging begins.

NEXT: [[Index]]
