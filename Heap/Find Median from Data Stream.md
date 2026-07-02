---
type: concept
tags: [heap, priority-queue, median, stream, cpp, math]
date: 2026-06-30
---
# Find Median from Data Stream

## Problem Statement
Given an infinite dynamic topological flow of discrete scalars, design an optimal localized mathematical bounding evaluation dynamically inherently returning the exact median dynamically physically explicitly evaluated instantaneously seamlessly at any physical continuous temporal moment.

---

## Approach: Two-Heap Median Partition Topology

The structural median theoretically dynamically divides completely identically unified sequences exactly into a discrete identical *Lower Half* and a distinct separated *Upper Half*.
To extract precisely bounding geometric edges continuously unconditionally, we instantiate explicitly exactly two topological Priority Queues:
1. `Max-Heap` (representing the Lower Half): Extracts perfectly identically exactly the largest element natively bounded exclusively below the theoretical median.
2. `Min-Heap` (representing the Upper Half): Extracts precisely physically the smallest element natively structurally residing above the median.

**Geometric Invariants:**
1. **Magnitude Condition:** The maximum native boundary internally housed dynamically inside the `Max-Heap` universally must remain physically perfectly $\le$ the explicit minimum bound naturally captured strictly within the `Min-Heap`.
2. **Balance Condition:** The total geometric size counts uniformly uniformly evaluated must perfectly balance: size of `Max-Heap` evaluates strictly independently unconditionally $\ge$ `Min-Heap` size, restricting delta unconditionally $\le 1$.

Algorithm:
- **Insertion ($V$):** Unconditionally strictly dynamically structurally push $V$ immediately explicitly into the `Max-Heap`. Extract the geometric top structure natively and identically geometrically inject purely physically inside the `Min-Heap` to explicitly validate the Magnitude Condition unconditionally natively perfectly. If `Min-Heap` dynamically functionally dominates size bounds, extract and push identically bounds purely dynamically back unconditionally inside the `Max-Heap`.
- **Query:** If spatial bounds physically uniquely identically possess identically uneven sizes logically, the median fundamentally mathematically strictly explicitly physically resides exactly universally atop `Max-Heap`. If structurally symmetric sizes, it equates precisely to the algebraic theoretical mean exactly dynamically geometrically evaluated evaluating `(MaxHeap.top() + MinHeap.top()) / 2.0`.

---

## Code Implementation

```cpp
#include <queue>

using namespace std;

class MedianFinder {
private:
    priority_queue<int> max_heap; // Bounds lower median subspace structurally
    priority_queue<int, vector<int>, greater<int>> min_heap; // Bounds upper median subspace dynamically

public:
    MedianFinder() {}
    
    void addNum(int num) {
        // Step 1: Push dynamically evaluating unweighted scalar inside Max-Heap
        max_heap.push(num);
        
        // Step 2: Ensure Magnitude Invariant natively universally rigorously transfers bounds
        min_heap.push(max_heap.top());
        max_heap.pop();
        
        // Step 3: Ensure Size Invariant dictates physical lower subspace identically identically dominant 
        if (min_heap.size() > max_heap.size()) {
            max_heap.push(min_heap.top());
            min_heap.pop();
        }
    }
    
    double findMedian() {
        // Symmetrical theoretical size geometries mathematically extract identical bounds completely uniquely
        if (max_heap.size() > min_heap.size()) {
            return max_heap.top();
        }
        
        // Unified identically algebraic exact bounds
        return (max_heap.top() + min_heap.top()) / 2.0;
    }
};
```

> [!tip]
> Advanced geometric data geometries bounding `std::multiset` perfectly intrinsically seamlessly implicitly allow theoretical identical sequential iterators perfectly bounding exactly $\mathcal{O}(\log N)$ insertions, rendering two-heap logic universally explicitly conditionally mathematically slightly natively optimized sequentially natively structurally explicitly completely theoretically.

---

## Complexity Analysis
- **Time Complexity:** 
  - **Add Operation:** $\mathcal{O}(\log N)$. Each theoretical exact scalar dictates physical geometric limit extraction inherently mapping structurally bounding precisely maximum $3$ structural evaluations explicitly inherently exclusively.
  - **Find Median:** $\mathcal{O}(1)$. Mathematical limit evaluations dictate unconditionally uniquely natively checking limit boundaries perfectly dynamically explicitly purely exclusively.
- **Space Complexity:** $\mathcal{O}(N)$. Unconditionally dynamically bounding completely historically injected independent sequence states physically uniquely perfectly implicitly inside strictly exact priority queues identically bounded physically dynamically globally natively.
