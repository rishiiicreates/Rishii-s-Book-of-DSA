---
type: concept
tags: [greedy, cpp, huffman-coding]
date: 2026-06-30
---
# Huffman Coding

## Problem Statement
Given a set of characters and their frequencies, generate a prefix-free binary code for each character that minimizes the total encoded length of the message.

## Approach / Intuition
Build a Huffman tree using a [[Priority Queue]] (min-heap). Extract the two nodes with the lowest frequencies, create a new internal node with a frequency equal to their sum, and push it back. Repeat until one node remains, which becomes the root. This is a classic [[Greedy Algorithm]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

struct Node {
    char data;
    unsigned freq;
    Node *left, *right;
    Node(char data, unsigned freq) : data(data), freq(freq), left(nullptr), right(nullptr) {}
};

struct compare {
    bool operator()(Node* l, Node* r) {
        return l->freq > r->freq;
    }
};

Node* buildHuffmanTree(vector<char>& chars, vector<unsigned>& freqs) {
    priority_queue<Node*, vector<Node*>, compare> minHeap;
    for (size_t i = 0; i < chars.size(); ++i) {
        minHeap.push(new Node(chars[i], freqs[i]));
    }
    while (minHeap.size() != 1) {
        Node *left = minHeap.top(); minHeap.pop();
        Node *right = minHeap.top(); minHeap.pop();
        Node *top = new Node('$', left->freq + right->freq);
        top->left = left;
        top->right = right;
        minHeap.push(top);
    }
    return minHeap.top();
}
```

## New Keywords / STL Used
- `priority_queue`, custom struct functor for comparison

## Edge Cases
- Only one character provided
- All characters have the same frequency
