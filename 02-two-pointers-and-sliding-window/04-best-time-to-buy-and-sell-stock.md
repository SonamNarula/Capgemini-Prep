# Best Time to Buy and Sell Stock

| | |
|---|---|
| **Section** | 2. Two Pointers & Sliding Window |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Running minimum |
| **Problem** | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

One buy then one later sell. Return the maximum profit, or `0` if no profit is possible.

**Clarify:** Is a single transaction allowed? Can I buy and sell the same day (profit 0)?

## 2. Brute Force

Try every buy day `i` and sell day `j > i`.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

For each sell day we only need the *cheapest earlier price*, not a scan of all earlier days.

## 5. Optimal Approach (and why it works)

One pass. Track `minPrice` seen so far; at each price `p`, profit if sold today is `p - minPrice`. Keep the max.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int maxProfit(vector<int>& prices) {
    int minPrice = INT_MAX, best = 0;
    for (int p : prices) {
        minPrice = min(minPrice, p);
        best = max(best, p - minPrice);
    }
    return best;
}
```

## 7. Dry Run

`[7, 1, 5, 3, 6, 4]`

- `7`: min 7, profit 0
- `1`: min 1, profit 0
- `5`: profit 4
- `3`: profit 2
- `6`: profit 5 (best)
- `4`: profit 3
- Answer `5`

## 8. Edge Cases

- Strictly decreasing `[7,6,4,3,1]` → `0`
- Single price → `0`
- All equal → `0`
- Empty array → `0`

## Likely Follow-ups

- Multiple transactions allowed (LC 122)? → Sum every positive consecutive difference.
- With cooldown or fee? → DP with states hold/sold/rest.
