# Time Complexities

## Theory
Time complexity measures how the runtime of an algorithm grows with input size. Common notations: O(1), O(n), O(log n), O(n^2).

## Example Code (Python)
```python
# O(1)
def constant_time(arr):
    return arr[0]
# O(n)
def linear_time(arr):
    for x in arr:
        print(x)
```

## Practice Problem
Classify the time complexity of different code snippets.

## Key Points
- Analyze best, average, and worst cases.
- Optimize for lower time complexity when possible.
