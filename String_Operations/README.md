# Advanced String Algorithms and Data Structures

## Theory

### Advanced String Algorithms
1. Pattern Matching
   - KMP Algorithm
   - Boyer-Moore Algorithm
   - Aho-Corasick Automaton
   - Suffix Arrays and Trees

2. String Distance
   - Levenshtein Distance
   - Damerau-Levenshtein
   - Longest Common Subsequence
   - Approximate String Matching

3. String Compression
   - Run-Length Encoding
   - Huffman Coding
   - Burrows-Wheeler Transform
   - Lempel-Ziv Compression

## Advanced Implementation

```python
from typing import List, Set, Dict, Optional, TypeVar
from collections import defaultdict
import array
from dataclasses import dataclass
import re
from functools import lru_cache

T = TypeVar('T')

class SuffixArray:
    """Efficient string pattern matching using suffix arrays."""
    
    def __init__(self, text: str):
        self.text = text
        self.n = len(text)
        self.suffix_array = self._build_suffix_array()
        self.lcp_array = self._build_lcp_array()
        
    def _build_suffix_array(self) -> List[int]:
        """Build suffix array using DC3/Skew algorithm."""
        suffixes = [(self.text[i:], i) for i in range(self.n)]
        suffixes.sort()
        return [suffix[1] for suffix in suffixes]
    
    def _build_lcp_array(self) -> List[int]:
        """Build LCP (Longest Common Prefix) array."""
        lcp = [0] * (self.n - 1)
        rank = [0] * self.n
        
        for i in range(self.n):
            rank[self.suffix_array[i]] = i
            
        k = 0
        for i in range(self.n):
            if rank[i] == self.n - 1:
                k = 0
                continue
                
            j = self.suffix_array[rank[i] + 1]
            while i + k < self.n and j + k < self.n and \
                  self.text[i + k] == self.text[j + k]:
                k += 1
                
            lcp[rank[i]] = k
            if k > 0:
                k -= 1
                
        return lcp

class AhoCorasick:
    """Efficient multiple pattern matching."""
    
    def __init__(self):
        self.dictionary = set()
        self.states = []
        self.fail = []
        self.output = []
        
    def add_pattern(self, pattern: str) -> None:
        self.dictionary.add(pattern)
        
    def build_automaton(self) -> None:
        """Build the automaton for fast pattern matching."""
        self.states = [{}]
        self.fail = [0]
        self.output = [set()]
        
        # Build trie and store patterns
        for pattern in self.dictionary:
            current = 0
            for char in pattern:
                if char not in self.states[current]:
                    self.states.append({})
                    self.fail.append(0)
                    self.output.append(set())
                    self.states[current][char] = len(self.states) - 1
                current = self.states[current][char]
            self.output[current].add(pattern)
            
        # Build failure function using BFS
        queue = []
        for char in self.states[0]:
            queue.append(self.states[0][char])
            
        while queue:
            current = queue.pop(0)
            for char in self.states[current]:
                queue.append(self.states[current][char])
                state = current
                while state and char not in self.states[self.fail[state]]:
                    state = self.fail[state]
                self.fail[self.states[current][char]] = \
                    self.states[self.fail[state]][char] if state else 0
                self.output[self.states[current][char]] |= \
                    self.output[self.fail[self.states[current][char]]]

class StringDistance:
    """Advanced string distance algorithms."""
    
    @staticmethod
    @lru_cache(maxsize=None)
    def levenshtein(s1: str, s2: str) -> int:
        """Compute Levenshtein distance with memoization."""
        if not s1: return len(s2)
        if not s2: return len(s1)
        
        if s1[0] == s2[0]:
            return StringDistance.levenshtein(s1[1:], s2[1:])
            
        return 1 + min(
            StringDistance.levenshtein(s1[1:], s2),    # deletion
            StringDistance.levenshtein(s1, s2[1:]),    # insertion
            StringDistance.levenshtein(s1[1:], s2[1:]) # substitution
        )
    
    @staticmethod
    def damerau_levenshtein(s1: str, s2: str) -> int:
        """Compute Damerau-Levenshtein distance."""
        d = {}
        len1, len2 = len(s1), len(s2)
        
        for i in range(-1, len1 + 1):
            d[(i, -1)] = i + 1
        for j in range(-1, len2 + 1):
            d[(-1, j)] = j + 1
            
        for i in range(len1):
            for j in range(len2):
                if s1[i] == s2[j]:
                    cost = 0
                else:
                    cost = 1
                    
                d[(i, j)] = min(
                    d[(i-1, j)] + 1,      # deletion
                    d[(i, j-1)] + 1,      # insertion
                    d[(i-1, j-1)] + cost  # substitution
                )
                
                if i > 0 and j > 0 and s1[i] == s2[j-1] and s1[i-1] == s2[j]:
                    d[(i, j)] = min(d[(i, j)], d[i-2, j-2] + cost) # transposition
                    
        return d[len1-1, len2-1]

class StringCompression:
    """Advanced string compression techniques."""
    
    @staticmethod
    def burrows_wheeler_transform(s: str) -> tuple[str, int]:
        """Perform Burrows-Wheeler transform."""
        s = s + '$'
        rotations = sorted([s[i:] + s[:i] for i in range(len(s))])
        last_chars = ''.join(rotation[-1] for rotation in rotations)
        original_idx = rotations.index(s)
        return last_chars, original_idx
    
    @staticmethod
    def inverse_burrows_wheeler(transformed: str, idx: int) -> str:
        """Recover original string from BWT."""
        table = [''] * len(transformed)
        for i in range(len(transformed)):
            table = sorted([c + table[j] for j, c in enumerate(transformed)])
        return table[idx][:-1]  # Remove sentinel
```

## Advanced Practice Problems

1. Implement a succinct suffix array
2. Create a compressed suffix tree
3. Design a fast approximate string matching algorithm
4. Build a real-time string indexing structure
5. Implement a string similarity search engine

## Performance Optimization

1. Memory Efficiency
   - Succinct data structures
   - Space-efficient indexing
   - Compression techniques

2. Algorithm Design
   - Suffix links
   - Skip techniques
   - Two-way search methods

3. Parallel Processing
   - SIMD string operations
   - Parallel pattern matching
   - Distributed string processing

## Real-world Applications

1. Text Processing
   - Search engines
   - DNA sequence analysis
   - Plagiarism detection

2. Data Compression
   - Text compression
   - Data deduplication
   - Archive formats

3. Information Retrieval
   - Full-text search
   - Spell checking
   - Auto-completion

## Interview Strategy
- Consider space-time tradeoffs
- Discuss preprocessing costs
- Address scalability
- Think about edge cases

## Advanced Concepts

1. Text Indexing
   - FM-index
   - Wavelet trees
   - q-gram indexes

2. Approximate Matching
   - Phonetic algorithms
   - Fuzzy string matching
   - Probabilistic matching

3. String Processing
   - Regular expressions
   - Context-free grammars
   - Parsing techniques

## Common Pitfalls
- Unicode handling
- Memory management
- Performance assumptions
- Edge case handling
