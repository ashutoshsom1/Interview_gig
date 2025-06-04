# Advanced Sorting Algorithms

## Theory

### Classical Algorithms with Advanced Optimizations
1. QuickSort Variants
   - Three-way partitioning
   - Dual-pivot implementation
   - Introspective sort (hybrid)
   
2. Merge Sort Optimizations
   - Bottom-up implementation
   - In-place merge sort
   - Parallel merge sort

3. Distribution Sorts
   - American flag sort
   - Burst sort
   - Flash sort

4. Specialized Sorts
   - Timsort (Python's built-in)
   - Pattern-defeating quicksort
   - Block merge sort (WikiSort)

## Advanced Implementations

```python
from typing import TypeVar, List, Callable, Optional
from abc import ABC, abstractmethod
import random
from concurrent.futures import ThreadPoolExecutor
import numpy as np

T = TypeVar('T')

class SortingAlgorithm(ABC, Generic[T]):
    @abstractmethod
    def sort(self, arr: List[T]) -> List[T]:
        pass

class DualPivotQuickSort(SortingAlgorithm[T]):
    def __init__(self, cutoff: int = 10):
        self.cutoff = cutoff

    def sort(self, arr: List[T]) -> List[T]:
        self._sort(arr, 0, len(arr) - 1)
        return arr

    def _sort(self, arr: List[T], left: int, right: int) -> None:
        if right - left < self.cutoff:
            self._insertion_sort(arr, left, right)
            return

        # Choose pivots
        if arr[left] > arr[right]:
            arr[left], arr[right] = arr[right], arr[left]
        
        p1, p2 = arr[left], arr[right]
        
        l, k, g = left + 1, left + 1, right - 1
        while k <= g:
            if arr[k] < p1:
                arr[l], arr[k] = arr[k], arr[l]
                l += 1
                k += 1
            elif arr[k] > p2:
                while arr[g] > p2 and k < g:
                    g -= 1
                arr[k], arr[g] = arr[g], arr[k]
                g -= 1
                if arr[k] < p1:
                    arr[l], arr[k] = arr[k], arr[l]
                    l += 1
            else:
                k += 1

        l -= 1
        g += 1
        arr[left], arr[l] = arr[l], arr[left]
        arr[right], arr[g] = arr[g], arr[right]

        self._sort(arr, left, l - 1)
        self._sort(arr, l + 1, g - 1)
        self._sort(arr, g + 1, right)

class ParallelMergeSort(SortingAlgorithm[T]):
    def __init__(self, num_threads: int = 4, threshold: int = 1000):
        self.num_threads = num_threads
        self.threshold = threshold

    def sort(self, arr: List[T]) -> List[T]:
        if len(arr) <= self.threshold:
            return sorted(arr)

        with ThreadPoolExecutor(max_workers=self.num_threads) as executor:
            mid = len(arr) // 2
            left = executor.submit(self.sort, arr[:mid])
            right = executor.submit(self.sort, arr[mid:])
            return self._merge(left.result(), right.result())

    def _merge(self, left: List[T], right: List[T]) -> List[T]:
        result = []
        i = j = 0
        
        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1
                
        result.extend(left[i:])
        result.extend(right[j:])
        return result

class AmericanFlagSort:
    """Linear time sort for integer keys with bounded range."""
    
    def __init__(self, max_value: int):
        self.max_value = max_value
        
    def sort(self, arr: List[int]) -> None:
        counts = [0] * (self.max_value + 1)
        temp = [0] * len(arr)
        
        # Count frequencies
        for x in arr:
            counts[x] += 1
            
        # Calculate positions
        total = 0
        for i in range(self.max_value + 1):
            count = counts[i]
            counts[i] = total
            total += count
            
        # Move elements
        for x in arr:
            temp[counts[x]] = x
            counts[x] += 1
            
        # Copy back
        arr[:] = temp
```

## Advanced Practice Problems

1. Implement a cache-oblivious sorting algorithm
2. Create a hybrid sorting algorithm that adapts to input patterns
3. Design a parallel external merge sort for large datasets
4. Implement a stable O(n) sorting algorithm for floating-point numbers
5. Create a sorting network implementation

## Performance Optimization Techniques

1. Memory Access Patterns
   - Cache line alignment
   - Sequential access optimization
   - Memory-efficient in-place sorting

2. Branch Prediction
   - Branchless implementations
   - Conditional move operations
   - Predicated instructions

3. SIMD Operations
   - Vectorized comparisons
   - Parallel merge operations
   - Bitonic merge networks

## Real-world Applications

1. Database Systems
   - Index creation
   - Query optimization
   - External sorting

2. Operating Systems
   - Process scheduling
   - Memory management
   - File system organization

3. Big Data Processing
   - Distributed sorting
   - Stream sorting
   - Near-real-time analytics

## Interview Strategy
- Discuss algorithm selection criteria
- Consider input characteristics
- Address scalability concerns
- Know optimization techniques

## Common Pitfalls
- Unstable sorting when stability matters
- Poor pivot selection in quicksort
- Memory allocation in merge sort
- Cache inefficiency in large arrays
