---
type: concept
tags: [heap, priority-queue, greedy, cpp, scheduling]
date: 2026-07-01
---
# Task Scheduler

## Problem Statement
Given an array of CPU tasks represented by characters ($A-Z$) and a cooling interval integer $n$, geometrically schedule the tasks to mathematically minimize the absolute execution cycles. The absolute constraint dictates that two identical topological tasks must be separated by strictly at least $n$ cycle boundaries (idle or distinct tasks). Output the mathematical minimum number of execution cycles.

---

## Approach: Max-Heap with Cooldown Queue (Greedy)

This is a classic greedy bottleneck mapping problem. To mathematically compress the absolute timeline, the tasks with the maximum structural frequency MUST be scheduled as aggressively as possible to prevent trailing idle permutations.

We implement a dual-structure topology:
1. A **Max-Heap (Priority Queue)** storing the absolute frequencies of all topologically valid tasks.
2. A **Cooldown Queue** simulating the strict temporal timeline, storing a tuple `{remaining_frequency, unlock_time}`.

In every absolute cycle (`time++`):
- If the Max-Heap is populated, extract the highest frequency task, mutate its count (`freq - 1`). If `freq > 0`, push it to the Cooldown Queue with an absolute unlock sequence `time + n`.
- Unconditionally check the temporal Cooldown Queue. If the absolute front element matches the current topological `time`, it is mathematically unlocked and re-injected into the Max-Heap.
- The temporal sequence mathematically terminates when BOTH structures are absolutely empty.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
#include <unordered_map>

using namespace std;

int leastInterval(vector<char>& tasks, int n) {
    // Structural frequency mapping
    unordered_map<char, int> freq_map;
    for (char task : tasks) {
        freq_map[task]++;
    }
    
    // Max-Heap bounds highest frequency
    priority_queue<int> max_heap;
    for (auto const& pair : freq_map) {
        max_heap.push(pair.second);
    }
    
    // Cooldown Queue: {remaining_freq, absolute_unlock_time}
    queue<pair<int, int>> cooldown;
    int current_time = 0;
    
    while (!max_heap.empty() || !cooldown.empty()) {
        current_time++; // Advance temporal boundary
        
        if (!max_heap.empty()) {
            int freq = max_heap.top();
            max_heap.pop();
            
            freq--; // Execute topological task
            
            // If structurally remaining, inject into cooldown horizon
            if (freq > 0) {
                cooldown.push({freq, current_time + n});
            }
        }
        
        // Re-inject unlocked tasks to active heap
        if (!cooldown.empty() && cooldown.front().second == current_time) {
            max_heap.push(cooldown.front().first);
            cooldown.pop();
        }
    }
    
    return current_time;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ absolute limit. The initial map construction operates in $O(N)$. The Max-Heap mathematically bounds at a maximum size of 26 (alphabet cardinality), reducing the priority queue operations to $O(1)$. Thus, processing tasks is strictly bounded by the scalar cycle temporal span.
- **Space Complexity:** $O(1)$ bounded constant. Both the Hash Map, Max-Heap, and Cooldown Queue mathematically peak at $O(26) \equiv O(1)$.

> [!important]
> **Mathematical Formula Alternative:** The temporal limit can be strictly algebraically evaluated without simulation: $\max(N, (\text{max\_freq} - 1) \times (n + 1) + \text{max\_freq\_count})$. While structurally $O(1)$, the Heap simulation remains architecturally critical for problems requiring the *exact sequence* of task execution.
