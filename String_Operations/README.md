# String Operations

## Theory
Strings are sequences of characters. Common operations: concatenation, slicing, searching, replacing, and formatting.

## Example Code (Python)
```python
s = "hello"
# Concatenation
s2 = s + " world"
# Slicing
sub = s[1:4]  # 'ell'
# Searching
found = "e" in s  # True
# Replacing
s3 = s.replace("l", "x")  # 'hexxo'
# Formatting
s4 = f"{s} world"
```

## Practice Problem
Reverse a string without using built-in reverse functions.

## Key Points
- Strings are immutable in Python.
- Efficient string handling is important for performance.
