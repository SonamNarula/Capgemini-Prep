# Longest Increasing Subsequence

| | |
|---|---|
| **Section** | 10. Dynamic Programming |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | DP / binary search |
| **Problem** | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the length of the longest **strictly increasing subsequence** (elements need not be contiguous).

**Clarify:** Strictly increasing? Return length only or the sequence too?

## 2. Brute Force

Generate all subsequences and check which are increasing.

## 3. Brute-Force Complexity

Time `O(2ⁿ · n)`.

## 4. Bottleneck

`O(n²)` DP (`dp[i] = 1 + max dp[j]` for `j < i`, `nums[j] < nums[i]`) is the baseline. The inner scan can be replaced by binary search.

## 5. Optimal Approach (and why it works)

Maintain `tails[k]` = smallest possible tail value of an increasing subsequence of length `k+1`. For each `x`, find the first tail `≥ x` with `lower_bound`: replace it with `x` (a better, smaller tail), or append if none (a longer subsequence exists). The size of `tails` is the LIS length. (`tails` is **not** the actual subsequence.)

**Complexity:** Time `O(n log n)`, Space `O(n)`. (Baseline DP: `O(n²)` time.)

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
// O(n log n)
int lengthOfLIS(vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), x);   // strictly increasing
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}

// O(n^2) DP: easier to explain first
int lengthOfLIS_dp(vector<int>& nums) {
    int n = nums.size(), best = 1;
    vector<int> dp(n, 1);
    for (int i = 1; i < n; i++)
        for (int j = 0; j < i; j++)
            if (nums[j] < nums[i]) dp[i] = max(dp[i], dp[j] + 1);
    for (int v : dp) best = max(best, v);
    return best;
}
```

## 7. Dry Run

`[10,9,2,5,3,7,101,18]`

- `10` → `[10]`; `9` → `[9]`; `2` → `[2]`
- `5` → `[2,5]`; `3` → `[2,3]`; `7` → `[2,3,7]`
- `101` → `[2,3,7,101]`; `18` → `[2,3,7,18]`
- Length `4`

## 8. Edge Cases

- Empty array → `0` (guard in DP version)
- Single element → `1`
- Strictly decreasing → `1`
- All equal → `1` (strict, so `lower_bound`, not `upper_bound`)
- Already increasing → `n`

## Likely Follow-ups

- Non-decreasing LIS? → Use `upper_bound`.
- Print the LIS? → Track predecessor indices.
- Related: Russian Doll Envelopes, number of LIS.
