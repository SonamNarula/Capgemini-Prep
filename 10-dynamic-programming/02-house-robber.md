# House Robber

| | |
|---|---|
| **Section** | 10. Dynamic Programming |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | State transition |
| **Problem** | [House Robber](https://leetcode.com/problems/house-robber/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Each house has money; you can't rob two adjacent houses. Maximise the total.

**Clarify:** Non-negative values? Houses in a line (not circle)?

## 2. Brute Force

Recursion: at each house choose to rob (skip next) or skip.

## 3. Brute-Force Complexity

Time `O(2ⁿ)`.

## 4. Bottleneck

Subproblems overlap: the best from index `i` onward is recomputed repeatedly.

## 5. Optimal Approach (and why it works)

`dp[i] = max(dp[i-1], dp[i-2] + nums[i])` (skip house `i`, or rob it plus the best up to `i-2`). Only the previous two states matter → `prev1`, `prev2`.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int rob(vector<int>& nums) {
    int prev2 = 0, prev1 = 0;                  // best up to i-2, best up to i-1
    for (int x : nums) {
        int cur = max(prev1, prev2 + x);
        prev2 = prev1;
        prev1 = cur;
    }
    return prev1;
}
```

## 7. Dry Run

`[2, 7, 9, 3, 1]` (track `prev2`, `prev1`)

- `x=2`: `cur = max(0, 0+2) = 2` → `prev2=0, prev1=2`
- `x=7`: `cur = max(2, 0+7) = 7` → `prev2=2, prev1=7`
- `x=9`: `cur = max(7, 2+9) = 11` → `prev2=7, prev1=11`
- `x=3`: `cur = max(11, 7+3) = 11` → `prev2=11, prev1=11`
- `x=1`: `cur = max(11, 11+1) = 12`
- Answer `12` (rob houses with `2 + 9 + 1`)

## 8. Edge Cases

- Empty array → `0`
- Single house
- Two houses → max of the two
- All zeros
- House Robber II (circular): run twice excluding first / last

## Likely Follow-ups

- Circular street (LC 213)?
- Why not greedy (take larger of adjacent)? → Counterexample `[2,1,1,2]`.
- Space optimisation from `dp[]`.
