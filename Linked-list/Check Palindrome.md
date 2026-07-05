# Check Palindrome

## Problem Statement
- given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.

## Approach / Intuition
- to check if a linked list is a palindrome in $O(1)$ space, we use [[Two Pointers]] (slow and fast) to find the middle of the list. We then reverse the second half of the list. Finally, we iterate through both halves simultaneously, comparing the values. If any values mismatch, it's not a palindrome. We can optionally reverse the second half back to restore the original list.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(1)$

## Sample Code
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* nextTemp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}

bool isPalindrome(ListNode* head) {
    if (!head || !head->next) return true;
    
    ListNode* slow = head;
    ListNode* fast = head;
    
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    
    slow->next = reverseList(slow->next);
    slow = slow->next;
    
    ListNode* dummy = head;
    while (slow) {
        if (dummy->val != slow->val) return false;
        dummy = dummy->next;
        slow = slow->next;
    }
    
    return true;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- empty list
- single element
- list with even length vs odd length

NEXT: [[Index]]
