# Counting Bits

| | |
|---|---|
| **Section** | 8. Bit Manipulation & Mathematics |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | DP + bit manipulation |
| **Problem** | [Counting Bits](https://leetcode.com/problems/counting-bits/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

For every `i` in `0..n`, return the number of `1` bits in `i`.

**Clarify:** Expected `O(n)` (not `O(n log n)`)?

## 2. Brute Force

Call a popcount routine for each `i` (`O(log i)` each).

## 3. Brute-Force Complexity

Time `O(n log n)`, Space `O(1)` extra.

## 4. Bottleneck

Counts for smaller numbers can be reused: `i` and `i >> 1` differ only by the last bit.

## 5. Optimal Approach (and why it works)

`bits[i] = bits[i >> 1] + (i & 1)`. Shifting right drops the last bit; add `1` if that bit was set.

**Complexity:** Time `O(n)`, Space `O(n)` (output).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> countBits(int n) {
    vector<int> dp(n + 1, 0);
    for (int i = 1; i <= n; i++)
        dp[i] = dp[i >> 1] + (i & 1);
    return dp;
}
```

## 7. Dry Run

`n = 5`

- `dp[0]=0`
- `dp[1] = dp[0] + 1 = 1`
- `dp[2] = dp[1] + 0 = 1`
- `dp[3] = dp[1] + 1 = 2`
- `dp[4] = dp[2] + 0 = 1`
- `dp[5] = dp[2] + 1 = 2`
- Result `[0,1,1,2,1,2]`

## 8. Edge Cases

- `n = 0` → `[0]`
- `n = 1` → `[0,1]`
- Powers of two → `1` bit
- Large `n` → fine, `O(n)` memory

## Likely Follow-ups

- Alternative recurrence? → `dp[i] = dp[i & (i-1)] + 1`.
- Why does it count as DP? → Each state reuses a smaller solved state.
