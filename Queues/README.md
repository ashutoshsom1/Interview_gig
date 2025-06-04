# Queues

## Theory
A queue is a linear data structure that follows the First-In-First-Out (FIFO) principle. Elements are added at the rear and removed from the front.

## Example Code (Python)
```python
from collections import deque
q = deque()
q.append(1)  # Enqueue
q.append(2)
print(q.popleft())  # Dequeue: 1
```

## Practice Problem
Implement a queue using two stacks.

## Key Points
- Used in scheduling, buffering, and breadth-first search.
- Python's `deque` is efficient for queue operations.
