# Single Number

| | |
|---|---|
| **Section** | 8. Bit Manipulation & Mathematics |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | XOR |
| **Problem** | [Single Number](https://leetcode.com/problems/single-number/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Every element appears twice except one. Find the single one in linear time and constant extra space.

**Clarify:** Exactly one unique element? Others exactly twice?

## 2. Brute Force

Count frequencies with a hash map and return the element with count 1.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

The extra map violates the constant-space requirement. We need an operation where equal values cancel.

## 5. Optimal Approach (and why it works)

XOR properties: `a ^ a = 0`, `a ^ 0 = a`, XOR is commutative and associative. XOR-ing all numbers cancels every pair, leaving the single number.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int singleNumber(vector<int>& nums) {
    int x = 0;
    for (int v : nums) x ^= v;
    return x;
}
```

## 7. Dry Run

`[4, 1, 2, 1, 2]`

- `0^4 = 4`; `4^1 = 5`; `5^2 = 7`; `7^1 = 6`; `6^2 = 4`
- Answer `4`

## 8. Edge Cases

- Single element → itself
- Negative numbers (XOR works on two's complement)
- Zero as the single number
- Order of elements doesn't matter

## Likely Follow-ups

- Every element appears 3 times except one (LC 137)? → Count bits mod 3.
- Two unique numbers (LC 260)? → XOR all, split by a set bit.
