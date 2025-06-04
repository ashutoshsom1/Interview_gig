# Linked Lists

## Theory
A linked list is a linear data structure where each element (node) contains a value and a reference (pointer) to the next node. Types: Singly, Doubly, Circular.

## Example Code (Python)
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Create nodes
a = Node(1)
b = Node(2)
a.next = b
```

## Practice Problem
Implement a function to reverse a singly linked list.

## Key Points
- Linked lists allow efficient insertions/deletions.
- No random access; must traverse from the head.
