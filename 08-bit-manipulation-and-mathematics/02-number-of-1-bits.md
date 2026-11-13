# Number of 1 Bits

| | |
|---|---|
| **Section** | 8. Bit Manipulation & Mathematics |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Brian Kernighan |
| **Problem** | [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Count the set bits (`1`s) in the binary representation of an unsigned 32-bit integer (Hamming weight).

**Clarify:** Unsigned? Negative inputs interpreted as their 32-bit pattern?

## 2. Brute Force

Check each of the 32 bits with `(n >> i) & 1` and count.

## 3. Brute-Force Complexity

Time `O(32)` = `O(1)` bits, Space `O(1)`.

## 4. Bottleneck

We always loop 32 times even if few bits are set. Kernighan's trick loops exactly once per set bit.

## 5. Optimal Approach (and why it works)

`n & (n - 1)` clears the **lowest set bit**. Repeat until `n` becomes `0`, counting iterations.

**Complexity:** Time `O(k)` where `k` = number of set bits, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int hammingWeight(uint32_t n) {
    int count = 0;
    while (n) {
        n &= (n - 1);                         // drop the lowest set bit
        count++;
    }
    return count;
}
// STL / builtin: __builtin_popcount(n) or std::bitset<32>(n).count()
```

## 7. Dry Run

`n = 11` (binary `1011`)

- `1011 & 1010 = 1010` → count 1
- `1010 & 1001 = 1000` → count 2
- `1000 & 0111 = 0000` → count 3
- Answer `3`

## 8. Edge Cases

- `n = 0` → `0`
- `n = 2^32 - 1` → `32`
- Power of two → `1`
- Use `uint32_t`: a signed `int` with `>>` is implementation-dependent for negatives

## Likely Follow-ups

- Check if a number is a power of two? → `n > 0 && (n & (n-1)) == 0`.
- Lookup-table approach for many queries?
