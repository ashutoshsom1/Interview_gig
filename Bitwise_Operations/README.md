# Bitwise Operations

## Theory
Bitwise operations work directly on the binary representation of integers. Common operations: AND (&), OR (|), XOR (^), NOT (~), left shift (<<), right shift (>>).

## Example Code (Python)
```python
a = 5      # 0b0101
b = 3      # 0b0011
print(a & b)  # AND: 1
print(a | b)  # OR: 7
print(a ^ b)  # XOR: 6
print(~a)     # NOT: -6
print(a << 1) # Left shift: 10
print(a >> 1) # Right shift: 2
```

## Practice Problem
Write a function to count the number of 1 bits in an integer.

## Key Points
- Bitwise operations are fast and memory-efficient.
- Useful in low-level programming, optimization, and algorithms.
