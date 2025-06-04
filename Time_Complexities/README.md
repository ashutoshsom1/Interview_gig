# Advanced Algorithm Analysis and Complexity Theory

## Theory

### Advanced Complexity Classes
1. P, NP, and NP-Complete
   - Cook-Levin theorem
   - Reduction techniques
   - Approximation algorithms

2. Space Complexity Classes
   - PSPACE and NPSPACE
   - L and NL classes
   - Memory hierarchy

3. Amortized Analysis
   - Aggregate method
   - Accounting method
   - Potential method

4. Probabilistic Analysis
   - Expected running time
   - Randomized algorithms
   - Las Vegas vs Monte Carlo

## Advanced Analysis Techniques

```python
from typing import List, TypeVar, Generic, Optional
from dataclasses import dataclass
import math
import random
from collections import deque

T = TypeVar('T')

class AmortizedExample:
    """Demonstrates amortized O(1) operations with occasional O(n) work."""
    
    def __init__(self, initial_capacity: int = 16):
        self.array = [None] * initial_capacity
        self.size = 0
        
    def add(self, item: T) -> None:
        # Amortized O(1) operation
        if self.size == len(self.array):
            # Double array size - O(n) but happens rarely
            new_array = [None] * (2 * len(self.array))
            for i in range(self.size):
                new_array[i] = self.array[i]
            self.array = new_array
        
        self.array[self.size] = item
        self.size += 1

class RandomizedQuickSelect:
    """Expected O(n) selection algorithm."""
    
    @staticmethod
    def select(arr: List[int], k: int) -> int:
        if not 0 <= k < len(arr):
            raise ValueError("k out of bounds")
            
        random.shuffle(arr)  # Randomization for expected O(n)
        return RandomizedQuickSelect._select(arr, 0, len(arr) - 1, k)
    
    @staticmethod
    def _select(arr: List[int], left: int, right: int, k: int) -> int:
        while True:
            pivot_idx = RandomizedQuickSelect._partition(arr, left, right)
            
            if pivot_idx == k:
                return arr[k]
            elif k < pivot_idx:
                right = pivot_idx - 1
            else:
                left = pivot_idx + 1

@dataclass
class ComplexityAnalysis:
    """Tool for analyzing algorithmic complexity."""
    
    name: str
    time_complexity: str
    space_complexity: str
    amortized_time: Optional[str] = None
    probabilistic_bound: Optional[str] = None
    
    def is_polynomial(self) -> bool:
        return not any(x in self.time_complexity.lower() 
                      for x in ['2^n', 'n!', 'n^n'])
    
    def is_practical(self, n: int) -> bool:
        if 'n^2' in self.time_complexity:
            return n <= 10000
        elif 'n log n' in self.time_complexity:
            return n <= 10000000
        elif 'n' in self.time_complexity:
            return n <= 100000000
        return True

# Example usage
sorting_analysis = ComplexityAnalysis(
    name="Quicksort",
    time_complexity="O(n log n)",
    space_complexity="O(log n)",
    amortized_time="O(n log n)",
    probabilistic_bound="O(n log n) with high probability"
)
```

## Advanced Analysis Problems

1. Prove the amortized cost of dynamic array resizing
2. Analyze the expected height of a randomized BST
3. Derive the competitive ratio of an online algorithm
4. Analyze the space-time tradeoff in dynamic programming

## Advanced Concepts

### Lower Bounds
- Comparison-based sorting
- Element distinctness
- Adversary arguments

### Competitive Analysis
- Online vs offline algorithms
- Competitive ratios
- Resource augmentation

### Complexity Hierarchies
- Time hierarchy theorem
- Space hierarchy theorem
- Relationships between classes

## Real-world Applications

1. Algorithm Engineering
   - Cache-aware analysis
   - Memory hierarchy effects
   - Parallel speedup analysis

2. Performance Prediction
   - Asymptotic behavior
   - Concrete performance models
   - Bottleneck identification

3. System Design
   - Scalability analysis
   - Resource utilization
   - Capacity planning

## Interview Strategy
- Start with asymptotic analysis
- Consider all complexity measures
- Discuss practical implications
- Know common complexity classes

## Advanced Topics

1. Fine-grained Complexity
   - Strong exponential time hypothesis
   - Reduction techniques
   - Conditional lower bounds

2. Parameterized Complexity
   - Fixed-parameter tractability
   - Kernelization
   - Parameterized reductions

3. Communication Complexity
   - Two-party protocols
   - Multi-party computation
   - Information complexity

## Common Pitfalls
- Ignoring constant factors
- Overlooking space complexity
- Missing amortized costs
- Incorrect recurrence relations
