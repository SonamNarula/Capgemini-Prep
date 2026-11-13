# Coin Change

| | |
|---|---|
| **Section** | 10. Dynamic Programming |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Unbounded knapsack |
| **Problem** | [Coin Change](https://leetcode.com/problems/coin-change/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given coin denominations (unlimited supply) and an `amount`, return the **fewest coins** needed, or `-1` if impossible.

**Clarify:** Positive denominations? `amount = 0` → `0`?

## 2. Brute Force

Recursively try each coin for the remaining amount.

## 3. Brute-Force Complexity

Time `O(Cᵃ)` exponential.

## 4. Bottleneck

Greedy (largest coin first) fails (e.g. coins `[1,3,4]`, amount `6`). Subproblems repeat heavily, so DP.

## 5. Optimal Approach (and why it works)

`dp[a]` = min coins for amount `a`. `dp[0] = 0`; `dp[a] = min(dp[a - c] + 1)` over coins `c ≤ a`. Initialise others to `amount + 1` (acts as infinity). If `dp[amount]` stays infinity → `-1`.

**Complexity:** Time `O(amount · coins)`, Space `O(amount)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int coinChange(vector<int>& coins, int amount) {
    const int INF = amount + 1;
    vector<int> dp(amount + 1, INF);
    dp[0] = 0;
    for (int a = 1; a <= amount; a++)
        for (int c : coins)
            if (c <= a)
                dp[a] = min(dp[a], dp[a - c] + 1);
    return dp[amount] >= INF ? -1 : dp[amount];
}
```

## 7. Dry Run

`coins = [1,2,5]`, `amount = 11`

- `dp = [0,1,1,2,2,1,2,2,3,3,2,3]`
- e.g. `dp[11] = min(dp[10], dp[9], dp[6]) + 1 = min(2,3,2) + 1 = 3` (`5+5+1`)

## 8. Edge Cases

- `amount = 0` → `0`
- Impossible `coins=[2]`, `amount=3` → `-1`
- Coin larger than amount
- Single coin that divides evenly
- Why greedy fails: `[1,3,4]`, `6` → `3+3` (2 coins), greedy gives `4+1+1` (3 coins)

## Likely Follow-ups

- Count the number of ways (Coin Change II)? → Loop coins outside, amounts inside.
- Memoised top-down version?
- Unbounded vs 0/1 knapsack difference?
