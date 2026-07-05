# Classes and Objects

## What are Classes and Objects?
- a **Class** is a user-defined blueprint or template that represents a real-world concept. It binds data (attributes/member variables) and the operations that manipulate that data (methods/member functions) into a single, cohesive unit. This is the core pillar of **Object-Oriented Programming (OOP)** known as **Encapsulation**.

- an **Object** is a concrete instance of a Class existing in memory.


## The Anatomy of a Class

```cpp
#include <iostream>
#include <string>
using namespace std;

class Node {
private:
    // Private attributes are hidden from the outside world.
    // They can only be modified by the class's own methods.
    int data;
    string name;

public:
    // 1. Constructor: Called automatically when the object is created
    // We use a Member Initializer List for maximum performance
    Node(int val, string n) : data(val), name(n) {
        cout << "Node created for " << name << "\n";
    }

    // 2. Member Function (Method)
    void printData() const {
        cout << name << " holds value: " << data << "\n";
    }

    // 3. Destructor: Called automatically when the object goes out of scope
    ~Node() {
        cout << "Node destroyed for " << name << "\n";
    }
};
```


## Using Objects

- to use the class, you instantiate it.
```cpp
int main() {
    // Creating an object on the Stack
    Node myNode(10, "Root");
    myNode.printData();
    
    // Creating an object on the Heap (Dynamic Allocation)
    Node* dynamicNode = new Node(20, "Child");
    dynamicNode->printData(); 
    
    // You MUST manually delete heap objects, or you will cause a memory leak
    delete dynamicNode; 
    
    return 0; // myNode's destructor is called automatically here
}
```

> [!important]
> **Stack vs Heap Allocation:** In modern C++, prefer Stack allocation (e.g. `Node n;`) whenever possible. It's significantly faster and the memory is managed entirely automatically. Only use Heap allocation (e.g. `new Node()`) when the object is massively large, needs to outlive the current function scope, or when building dynamic linked structures (like Trees and Linked Lists).


## `struct` vs `class` in C++

- in languages like C#, structs are fundamentally different from classes (value type vs reference type). However, in C++, **a struct and a class are exactly the same thing**, with only one tiny difference:
- in a `class`, all members are **private** by default.
- in a `struct`, all members are **public** by default.

### When to use which?
- in Competitive Programming and DSA:
- use **`struct`** for Plain Old Data structures (like a point in 2D space, a node in a Linked List, or a graph edge) where you just want to group variables together and access them directly without writing tedious getter/setter methods.
- use **`class`** when building a complex system that requires strict encapsulation and data protection.

```cpp
// Common DSA pattern: using a struct for a graph Edge
struct Edge {
    int to;
    int weight;
    
    // Constructor inside a struct
    Edge(int t, int w) : to(t), weight(w) {} 
};
```


## Complexity
- **time Complexity:** Object creation takes time proportional to the logic inside its constructor. For a simple initialization, it's $O(1)$.
- **space Complexity:** $O(S)$, where $S$ is the sum of the byte sizes of all member variables inside the class. Methods (functions) do not take up per-object space; they exist once in the executable code segment.

NEXT: [[Index]]
