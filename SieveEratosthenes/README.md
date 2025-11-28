# Sieve of Eratosthenes

## Prime Number Generation Algorithm

## Overview

A Java implementation of the Sieve of Eratosthenes, an ancient and efficient algorithm for finding all prime numbers up to a specified limit. This implementation finds all primes up to 1000.

## Author

- **Anmol Dubey**

## Algorithm

The Sieve of Eratosthenes works by iteratively marking the multiples of each prime number as composite (non-prime), starting from 2.

### Process

```
1. Create boolean array [2..n], all initially TRUE (assumed prime)
2. Start with first prime p = 2
3. Mark all multiples of p (p², p²+p, p²+2p, ...) as FALSE
4. Find next TRUE value, set as new p
5. Repeat until p² > n
6. All remaining TRUE indices are prime
```

### Visual Example (n = 30)

```
Initial:  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
          T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T  T

Cross 2s: 2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
          T  T  x  T  x  T  x  T  x  T  x  T  x  T  x  T  x  T  x  T  x  T  x  T  x  T  x  T  x

Cross 3s: 2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
          T  T  x  T  x  T  x  x  x  T  x  T  x  x  x  T  x  T  x  x  x  T  x  T  x  x  x  T  x

Cross 5s: 2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
          T  T  x  T  x  T  x  x  x  T  x  T  x  x  x  T  x  T  x  x  x  T  x  x  x  x  x  T  x

Primes:   2  3     5     7          11    13          17    19          23                29
```

### Why Start at p²?

The algorithm marks multiples starting from `p²` rather than `2p` because all smaller multiples of `p` have already been marked by smaller primes.

```
For p = 5:
- 2×5 = 10 already marked by 2
- 3×5 = 15 already marked by 3  
- 4×5 = 20 already marked by 2
- 5×5 = 25 ← first unmarked multiple
```

## Implementation

```java
public class SieveSolver {
    public static void main(String args[]) {
        int num = 1000;
        boolean prime[] = new boolean[num + 1];
        
        // Initialize all as prime
        for(int i = 0; i < num; i++)
            prime[i] = true;
        
        // Sieve: mark composites as false
        for(int j = 2; (j * j) <= num; j++) {
            if(prime[j] == true) {
                for(int i = j * j; i <= num; i += j)
                    prime[i] = false;
            }
        }
        
        // Print all primes
        for(int i = 2; i <= num; i++) {
            if(prime[i] == true)
                System.out.println(i + " ");
        }
    }
}
```

## File Structure

```
SieveEratosthenes/
├── README.md                    # This file
├── SieveEratosthenes.iml       # IntelliJ project config
└── src/
    └── SieveSolver.java        # Main implementation
```

## Usage

### Compile and Run

```bash
cd src
javac SieveSolver.java
java SieveSolver
```

### Output (First 25 Primes)

```
2 
3 
5 
7 
11 
13 
17 
19 
23 
29 
31 
37 
41 
43 
47 
...
```

## Complexity Analysis

| Metric | Complexity |
|--------|------------|
| Time | O(n log log n) |
| Space | O(n) |

The time complexity derives from the harmonic series of primes: each prime p marks n/p composites, and the sum over all primes is O(n log log n).

## Prime Count Results

| Range | Prime Count |
|-------|-------------|
| 1-100 | 25 |
| 1-1000 | 168 |
| 1-10000 | 1229 |

## Historical Context

The Sieve of Eratosthenes was invented by Eratosthenes of Cyrene (c. 276–194 BC), a Greek mathematician and chief librarian at the Library of Alexandria. It remains one of the most efficient methods for finding all primes below a moderate limit.

## Dependencies

- Java SE 8 or higher

