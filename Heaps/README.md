# Heaps

## Theory
A heap is a special tree-based data structure that satisfies the heap property: in a max heap, every parent node is greater than or equal to its children; in a min heap, every parent is less than or equal to its children.

## Example Code (Python)
```python
import heapq
heap = []
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 2)
print(heapq.heappop(heap))  # 1 (min-heap)
```

## Practice Problem
Implement a max heap using Python's list.

## Key Points
- Heaps are used in priority queues and heap sort.
- Python's `heapq` implements a min-heap by default.
