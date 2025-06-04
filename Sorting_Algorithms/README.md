# Sorting Algorithms

## Theory
Sorting algorithms arrange elements in a specific order (ascending/descending). Common algorithms: Insertion, Selection, Merge, Quick sort.

## Example Code (Python)
```python
# Insertion Sort
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```

## Practice Problem
Implement merge sort and quick sort.

## Key Points
- Know time/space complexities.
- Choose the right algorithm for the problem size and constraints.
