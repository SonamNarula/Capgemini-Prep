# Koko Eating Bananas

| | |
|---|---|
| **Section** | 4. Binary Search |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Binary search on answer |
| **Problem** | [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Piles of bananas, `h` hours. Each hour Koko eats up to `k` bananas from one pile. Find the minimum integer `k` that finishes all piles within `h` hours.

**Clarify:** `h ≥ number of piles`? (Guaranteed.) Large pile sizes (up to `1e9`)?

## 2. Brute Force

Try `k = 1, 2, 3, …` until the total hours ≤ `h`.

## 3. Brute-Force Complexity

Time `O(maxPile · n)`, Space `O(1)`.

## 4. Bottleneck

Hours needed is **monotonic** in `k` (bigger `k` → fewer hours), so we can binary search the answer instead of trying each value.

## 5. Optimal Approach (and why it works)

Search space `k ∈ [1, max(piles)]`. For a candidate `k`, hours = `Σ ceil(pile / k)`. If hours ≤ `h`, `k` is feasible → try smaller (`hi = mid`); otherwise `lo = mid + 1`. Use `(p - 1) / k + 1` for ceil to avoid overflow, and `long long` for the sum.

**Complexity:** Time `O(n log maxPile)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int minEatingSpeed(vector<int>& piles, int h) {
    int lo = 1, hi = *max_element(piles.begin(), piles.end());
    auto hoursNeeded = [&](int k) {
        long long t = 0;
        for (int p : piles) t += (p - 1) / k + 1;   // ceil(p / k), overflow safe
        return t;
    };
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (hoursNeeded(mid) <= h) hi = mid;        // feasible: try slower
        else lo = mid + 1;                          // too slow
    }
    return lo;
}
```

## 7. Dry Run

`piles = [3,6,7,11]`, `h = 8`

- `lo=1, hi=11`: `mid=6` → hours `1+1+2+2=6 ≤ 8` → `hi=6`
- `mid=3` → `1+2+3+4=10 > 8` → `lo=4`
- `mid=5` → `1+2+2+3=8 ≤ 8` → `hi=5`
- `mid=4` → `1+2+2+3=8 ≤ 8` → `hi=4`
- `lo=hi=4` → answer `4`

## 8. Edge Cases

- `h == piles.size()` → answer is `max(piles)`
- Single pile
- Huge piles (`1e9`) → `long long` for hours
- `h` very large → answer `1`
- Piles of size 1

## Likely Follow-ups

- Template for any 'minimise the maximum' / 'smallest feasible value' problem? → Yes: monotonic `feasible(x)` + binary search.
- Why `lo < hi` and `hi = mid`? → Finds the *first* feasible value.
