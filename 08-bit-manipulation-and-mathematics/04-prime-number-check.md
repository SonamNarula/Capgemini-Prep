# Prime Number Check

| | |
|---|---|
| **Section** | 8. Bit Manipulation & Mathematics |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Divisors up to square root |
| **Problem** | Prime Number Check *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Decide whether `n` is prime.

**Clarify:** Range of `n` (up to `1e9`/`1e12`)? Is `1` prime? (No.) Many queries (sieve) or a single one?

## 2. Brute Force

Test divisibility by every number from `2` to `n-1`.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(1)`.

## 4. Bottleneck

Divisors come in pairs `(d, n/d)`; one is always `≤ √n`. Checking beyond `√n` is redundant.

## 5. Optimal Approach (and why it works)

Handle `n < 2`, `2`, `3`, multiples of 2 and 3. Then test `i = 5, 11, 17, …` and `i + 2` (numbers of form `6k ± 1`) while `i * i <= n`.

**Complexity:** Time `O(√n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
bool isPrime(long long n) {
    if (n < 2) return false;
    if (n < 4) return true;                    // 2 and 3
    if (n % 2 == 0 || n % 3 == 0) return false;
    for (long long i = 5; i * i <= n; i += 6)  // test 6k-1 and 6k+1
        if (n % i == 0 || n % (i + 2) == 0) return false;
    return true;
}
```

## 7. Dry Run

`n = 29`

- Not divisible by 2 or 3
- `i=5`: `25 ≤ 29` → `29 % 5 ≠ 0`, `29 % 7 ≠ 0`
- `i=11`: `121 > 29` → stop → `true`

## 8. Edge Cases

- `0`, `1`, negatives → `false`
- `2`, `3` → `true`
- Perfect squares of primes `49` (caught at `i=7` since `7*7 <= 49`)
- Large prime near `1e12` → use `long long`, `i*i` safe up to `~9e18`
- Even numbers > 2

## Likely Follow-ups

- Many queries? → Sieve of Eratosthenes.
- Very large (`1e18`)? → Miller-Rabin.
