# Advanced Hash Maps and Hash Tables

## Theory
Hash tables are complex data structures with various implementation strategies and optimizations:

### Advanced Concepts
1. Hash Functions
   - Cryptographic vs. Non-cryptographic
   - Universal hashing
   - Perfect hashing
   - Locality-sensitive hashing

2. Collision Resolution
   - Open addressing variants
     * Linear probing
     * Quadratic probing
     * Double hashing
   - Separate chaining with balanced trees
   - Cuckoo hashing
   - Robin Hood hashing

3. Dynamic Resizing
   - Load factor analysis
   - Incremental resizing
   - Split-ordered lists

## Advanced Implementation

```python
from typing import Generic, TypeVar, Optional, List
from abc import ABC, abstractmethod
import mmh3  # MurmurHash3 for better distribution

K = TypeVar('K')
V = TypeVar('V')

class HashFunction(ABC):
    @abstractmethod
    def hash(self, key: K, seed: int = 0) -> int:
        pass

class MurmurHash(HashFunction):
    def hash(self, key: K, seed: int = 0) -> int:
        return mmh3.hash(str(key), seed)

class RobinHoodHashMap(Generic[K, V]):
    def __init__(self, initial_size: int = 16):
        self.size = initial_size
        self.count = 0
        self.table = [(None, None, -1) for _ in range(initial_size)]  # (key, value, PSL)
        self.hash_fn = MurmurHash()
        
    def _probe_distance(self, hash_val: int, current_pos: int) -> int:
        return (current_pos - hash_val) % self.size
        
    def insert(self, key: K, value: V) -> None:
        if self.count >= self.size * 0.7:  # Load factor threshold
            self._resize()
            
        hash_val = self.hash_fn.hash(key) % self.size
        current = (key, value, 0)  # PSL = 0
        pos = hash_val
        
        while True:
            if self.table[pos][0] is None:
                self.table[pos] = current
                self.count += 1
                return
                
            # Robin Hood: swap if current entry has higher PSL
            if self._probe_distance(hash_val, pos) > self.table[pos][2]:
                current, self.table[pos] = self.table[pos], current
                
            pos = (pos + 1) % self.size
```

## Advanced Practice Problems

1. Implement a concurrent hash map with fine-grained locking
2. Create a persistent hash map with O(1) snapshots
3. Design a distributed hash table with consistent hashing
4. Build a cache-oblivious hash table
5. Implement a perfect hash function for static keys

## Performance Optimization Techniques

1. Memory Layout
   - Cache line alignment
   - Grouped storage
   - Memory pooling

2. Algorithmic Improvements
   - Branch reduction
   - SIMD operations
   - Bloom filter pre-filtering

3. Concurrent Access
   - Lock-free algorithms
   - Read-copy-update (RCU)
   - Epoch-based reclamation

## Real-world Applications

1. Database Systems
   - Index structures
   - Query optimization
   - Join algorithms

2. Caching Systems
   - LRU cache implementation
   - Distributed caching
   - Write-through vs write-back

3. Network Systems
   - Routing tables
   - Flow tables
   - Connection tracking

## Interview Strategy
- Discuss tradeoffs between different collision resolution strategies
- Consider memory vs CPU usage
- Think about concurrent access patterns
- Address scalability concerns

## Common Pitfalls
- Poor hash function distribution
- Ignoring load factor implications
- Inadequate collision handling
- Race conditions in concurrent access
- Memory leaks in custom implementations
