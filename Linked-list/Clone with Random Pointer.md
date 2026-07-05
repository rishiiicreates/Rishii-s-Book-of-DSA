# Clone List with Random Pointer

## Problem Statement
- a linked list is given such that each explicit node natively contains an additional random pointer which could actively point to any node structurally in the list or strictly to null. Return a deep physical copy of the list.

## Approach / Intuition
- interweave original nodes and identical clone nodes structurally together sequentially (`A -> A' -> B -> B'`). Set random pointers dynamically on the cloned nodes relying upon references locked in the interwoven nodes, achieving flawless [[Memory Mapping]]. Finally, decouple both lists sequentially.

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(1) auxiliary

## Sample Code
```cpp
Node* copyRandomList(Node* head) {
    if (!head) return nullptr;
    Node* curr = head;
    while (curr) {
        Node* nextNode = curr->next;
        curr->next = new Node(curr->val);
        curr->next->next = nextNode;
        curr = nextNode;
    }
    curr = head;
    while (curr) {
        if (curr->random) {
            curr->next->random = curr->random->next;
        }
        curr = curr->next->next;
    }
    curr = head;
    Node* copyHead = head->next;
    Node* copyCurr = copyHead;
    while (curr) {
        curr->next = curr->next->next;
        if (copyCurr->next) {
            copyCurr->next = copyCurr->next->next;
        }
        curr = curr->next;
        copyCurr = copyCurr->next;
    }
    return copyHead;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- massive arrays of random pointers targeting identical nodes locally, fully null original structures gracefully exiting.

NEXT: [[Index]]
