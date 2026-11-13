# Sieve of Eratosthenes

| | |
|---|---|
| **Section** | 8. Bit Manipulation & Mathematics |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Boolean sieve |
| **Problem** | Sieve of Eratosthenes *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

List all primes up to `n`.

**Clarify:** Upper limit of `n` (memory)? Need a boolean table or list?

## 2. Brute Force

Run an `O(√x)` primality test for each number `2..n`.

## 3. Brute-Force Complexity

Time `O(n√n)`, Space `O(1)`.

## 4. Bottleneck

Each number is tested independently; composites can be marked in bulk from their prime factors.

## 5. Optimal Approach (and why it works)

Assume all numbers are prime. For each prime `i` with `i * i <= n`, mark multiples `i*i, i*i+i, …` as composite (smaller multiples were already marked by smaller primes). Whatever remains unmarked is prime.

**Complexity:** Time `O(n log log n)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> sieve(int n) {
    if (n < 2) return {};
    vector<bool> isP(n + 1, true);
    isP[0] = isP[1] = false;
    for (long long i = 2; i * i <= n; i++)
        if (isP[i])
            for (long long j = i * i; j <= n; j += i)
                isP[j] = false;
    vector<int> primes;
    for (int i = 2; i <= n; i++)
        if (isP[i]) primes.push_back(i);
    return primes;
}
```

## 7. Dry Run

`n = 20`

- `i=2` → marks `4,6,8,…,20`
- `i=3` → marks `9,12,15,18`
- `i=4` → already composite
- `i=5`: `25 > 20` stop
- Primes `2 3 5 7 11 13 17 19`

## 8. Edge Cases

- `n < 2` → empty list
- `n = 2` → `{2}`
- Start inner loop at `i*i` (not `2*i`), using `long long` to avoid overflow in `i*i`
- Memory for `n = 1e7` is fine with `vector<bool>`, `1e9` needs a segmented sieve

## Likely Follow-ups

- Count primes (LC 204)? → Same sieve.
- Smallest prime factor sieve for fast factorisation?
- Segmented sieve for a range `[L, R]`.
