# Data Types

## Theory
Data types are fundamental building blocks that define how data is stored in memory and what operations can be performed on it. Advanced concepts include:
- Memory representation and bit patterns
- Type conversion and coercion
- Custom data types and type hints
- Generic types and type variables

## Example Code (Python)
```python
from typing import TypeVar, Generic, List, Optional
from dataclasses import dataclass

# Advanced type hints
T = TypeVar('T')

@dataclass
class Container(Generic[T]):
    data: T
    metadata: Optional[dict] = None

# Custom data type with slots for memory efficiency
class Point:
    __slots__ = ['x', 'y']
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

# Type conversion and bit representation
def analyze_number(n: int) -> tuple[str, str, int]:
    """Return (hex, binary, number of bits) for an integer."""
    return hex(n), bin(n), n.bit_length()

# Memory views and byte arrays
def memory_view_example(data: bytes) -> None:
    view = memoryview(data)
    print(f"Size in memory: {view.nbytes} bytes")
    print(f"Item size: {view.itemsize} bytes")
    print(f"Format: {view.format}")
```

## Practice Problems
1. Implement a fixed-point decimal type with specified precision
2. Create a custom string type that maintains a history of all modifications
3. Write a function that determines if two variables refer to the same object in memory
4. Implement a binary-to-decimal converter using bitwise operations

## Advanced Concepts
- Memory layout and alignment
- Reference counting and garbage collection
- Type inference systems
- Immutable vs mutable types

## Interview Tips
- Understand memory implications of different types
- Know when to use immutable types for thread safety
- Be familiar with type coercion rules
- Consider performance implications of boxing/unboxing

## Common Pitfalls
- Integer overflow/underflow
- Floating-point precision errors
- Mutable default arguments
- Reference vs value comparisons

## Real-world Applications
- Database field type optimization
- Network protocol serialization
- Memory-efficient data structures
- Type-safe APIs
