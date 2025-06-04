# Advanced Bit Manipulation

## Theory

### Advanced Bit Operations
1. SIMD (Single Instruction Multiple Data)
   - Parallel bit operations
   - Vector instructions
   - AVX/SSE optimizations

2. Advanced Bit Tricks
   - Bit twiddling hacks
   - Population count algorithms
   - Branch-free programming
   - Sparse bit operations

3. Hardware-Level Optimizations
   - CPU-specific instructions
   - Carry-less multiplication
   - Bit field extraction
   - Fast bit reversal

## Advanced Implementation

```python
from typing import List, Tuple, Set
import array
import ctypes
import math
from functools import lru_cache

class BitManipulation:
    """Advanced bit manipulation techniques."""
    
    @staticmethod
    def parallel_bits_count(x: int) -> int:
        """Population count using parallel computation."""
        # Hamming weight - parallel bits counting
        x = x - ((x >> 1) & 0x5555555555555555)
        x = (x & 0x3333333333333333) + ((x >> 2) & 0x3333333333333333)
        x = (x + (x >> 4)) & 0x0F0F0F0F0F0F0F0F
        return (x * 0x0101010101010101) >> 56

    @staticmethod
    def isolate_rightmost_one(x: int) -> int:
        """Extract rightmost set bit."""
        return x & -x

    @staticmethod
    def next_power_of_2(x: int) -> int:
        """Find next power of 2 using bit manipulation."""
        x -= 1
        x |= x >> 1
        x |= x >> 2
        x |= x >> 4
        x |= x >> 8
        x |= x >> 16
        x |= x >> 32
        return x + 1

class BitVector:
    """Memory-efficient bit vector with advanced operations."""
    
    def __init__(self, size: int):
        self.size = size
        self.bits = array.array('Q', [0] * ((size + 63) // 64))
        
    def set_bit(self, pos: int) -> None:
        if 0 <= pos < self.size:
            word_idx = pos // 64
            bit_idx = pos % 64
            self.bits[word_idx] |= (1 << bit_idx)
            
    def clear_bit(self, pos: int) -> None:
        if 0 <= pos < self.size:
            word_idx = pos // 64
            bit_idx = pos % 64
            self.bits[word_idx] &= ~(1 << bit_idx)
            
    def toggle_bit(self, pos: int) -> None:
        if 0 <= pos < self.size:
            word_idx = pos // 64
            bit_idx = pos % 64
            self.bits[word_idx] ^= (1 << bit_idx)
            
    def get_bit(self, pos: int) -> bool:
        if 0 <= pos < self.size:
            word_idx = pos // 64
            bit_idx = pos % 64
            return bool(self.bits[word_idx] & (1 << bit_idx))
        return False

    def count_ones(self) -> int:
        """Count set bits using CPU's native POPCNT when available."""
        return sum(bin(x).count('1') for x in self.bits)

    def next_set_bit(self, pos: int) -> int:
        """Find next set bit after given position."""
        if pos >= self.size:
            return -1
            
        word_idx = pos // 64
        bit_idx = pos % 64
        
        # Check current word
        word = self.bits[word_idx] & (~0 << bit_idx)
        while word_idx < len(self.bits):
            if word:
                return word_idx * 64 + word.bit_length() - 1
            word_idx += 1
            if word_idx < len(self.bits):
                word = self.bits[word_idx]
        return -1

class GrayCode:
    """Gray code operations and utilities."""
    
    @staticmethod
    def binary_to_gray(n: int) -> int:
        """Convert binary number to Gray code."""
        return n ^ (n >> 1)
    
    @staticmethod
    def gray_to_binary(n: int) -> int:
        """Convert Gray code to binary number."""
        mask = n
        while mask:
            mask >>= 1
            n ^= mask
        return n
    
    @staticmethod
    @lru_cache(maxsize=None)
    def generate_gray_sequence(n: int) -> List[int]:
        """Generate n-bit Gray code sequence."""
        if n <= 0:
            return [0]
        
        # Generate sequence recursively
        lower = GrayCode.generate_gray_sequence(n - 1)
        return lower + [x | (1 << (n-1)) for x in reversed(lower)]

class BitMatrix:
    """Bit matrix with efficient operations."""
    
    def __init__(self, rows: int, cols: int):
        self.rows = rows
        self.cols = cols
        self.data = [BitVector(cols) for _ in range(rows)]
    
    def set(self, row: int, col: int, value: bool) -> None:
        if 0 <= row < self.rows and 0 <= col < self.cols:
            if value:
                self.data[row].set_bit(col)
            else:
                self.data[row].clear_bit(col)
                
    def get(self, row: int, col: int) -> bool:
        if 0 <= row < self.rows and 0 <= col < self.cols:
            return self.data[row].get_bit(col)
        return False
    
    def row_and(self, row1: int, row2: int) -> None:
        """Perform bitwise AND between two rows."""
        if 0 <= row1 < self.rows and 0 <= row2 < self.rows:
            for i in range(len(self.data[row1].bits)):
                self.data[row1].bits[i] &= self.data[row2].bits[i]
```

## Advanced Practice Problems

1. Implement a lock-free bitmap
2. Create a parallel bit counting algorithm
3. Design a bit-sliced implementation of AES
4. Build a bit-parallel pattern matching algorithm
5. Implement a succinct data structure

## Performance Optimization

1. CPU Architecture
   - Instruction pipelining
   - Branch prediction
   - Cache alignment
   - SIMD utilization

2. Algorithm Design
   - Branch elimination
   - Lookup table optimization
   - Parallel processing
   - Bit-parallel algorithms

3. Memory Efficiency
   - Compact representations
   - Succinct data structures
   - Cache-conscious layouts

## Real-world Applications

1. Cryptography
   - Block ciphers
   - Hash functions
   - Random number generation

2. Network Programming
   - IP address manipulation
   - Packet processing
   - Network masks

3. Graphics Processing
   - Color manipulation
   - Texture compression
   - Fast transforms

## Interview Strategy
- Consider hardware implications
- Discuss optimization techniques
- Address portability issues
- Think about scalability

## Advanced Concepts

1. Advanced Bit Fields
   - Dynamic bit fields
   - Bit field extraction
   - Field manipulation

2. Specialized Operations
   - Carry-less multiplication
   - Bit interleaving
   - Perfect hashing

3. Hardware Features
   - CPU specific instructions
   - Vector operations
   - Hardware accelerators

## Common Pitfalls
- Platform dependencies
- Sign extension issues
- Undefined behavior
- Performance assumptions
