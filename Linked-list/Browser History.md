# Design Browser History

## Problem Statement
- design a browser history system that allows you to cleanly visit URLs, go back, and go forward by a specific number of steps.

## Approach / Intuition
- implement a doubly linked list where each structural node strictly stores a URL. Maintain a pointer to the active page. Moving backward and forward merely requires traversing the `prev` and `next` pointers. Visiting a new page automatically clears all forward history safely, emphasizing effective [[Doubly Linked List]] manipulation.

## Time & Space Complexity
- **[[time Complexity]]:** O(1) for visit, O(Steps) for back/forward
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
class BrowserHistory {
    struct Node {
        string url;
        Node* prev;
        Node* next;
        Node(string u) : url(u), prev(nullptr), next(nullptr) {}
    };
    Node* curr;
public:
    BrowserHistory(string homepage) {
        curr = new Node(homepage);
    }
    void visit(string url) {
        Node* node = new Node(url);
        curr->next = node;
        node->prev = curr;
        curr = node;
    }
    string back(int steps) {
        while (curr->prev && steps--) {
            curr = curr->prev;
        }
        return curr->url;
    }
    string forward(int steps) {
        while (curr->next && steps--) {
            curr = curr->next;
        }
        return curr->url;
    }
};
```

## New Keywords / STL Used
- `struct`

## Edge Cases
- stepping back significantly further than the historical capacity, stepping forward past the most recent page boundary.

NEXT: [[Index]]
