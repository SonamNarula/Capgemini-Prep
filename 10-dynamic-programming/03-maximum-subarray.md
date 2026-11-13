# Maximum Subarray

| | |
|---|---|
| **Section** | 10. Dynamic Programming |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Kadane's algorithm |
| **Problem** | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Find the contiguous subarray (at least one element) with the largest sum.

**Clarify:** Can all numbers be negative? (Yes, so the answer may be negative.) Return the sum or the indices?

## 2. Brute Force

Compute the sum of every subarray with two (or three) nested loops.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

Recomputed sums. A running best-ending-here value is all we need.

## 5. Optimal Approach (and why it works)

Kadane: `cur` = best sum of a subarray **ending** at `i`: either extend (`cur + x`) or restart at `x`. `best = max(best, cur)`. Initialise both with `nums[0]`, **not** `0`, so all-negative arrays work.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int maxSubArray(vector<int>& nums) {
    int cur = nums[0], best = nums[0];
    for (int i = 1; i < (int)nums.size(); i++) {
        cur = max(nums[i], cur + nums[i]);     // extend or restart
        best = max(best, cur);
    }
    return best;
}
```

## 7. Dry Run

`[-2,1,-3,4,-1,2,1,-5,4]`

- `cur`: `-2, 1, -2, 4, 3, 5, 6, 1, 5`
- `best` ends at `6` (subarray `[4,-1,2,1]`)

## 8. Edge Cases

- All negative `[-3,-1,-2]` → `-1` (do **not** initialise with `0`)
- Single element
- All positive → whole array
- Zeros
- Large values → `long long` if needed

## Likely Follow-ups

- Return the actual subarray? → Track `start` when restarting.
- Circular maximum subarray (LC 918)?
- Maximum product subarray (LC 152)?
