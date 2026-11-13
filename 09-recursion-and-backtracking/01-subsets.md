# Subsets

| | |
|---|---|
| **Section** | 9. Recursion & Backtracking |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Include/exclude |
| **Problem** | [Subsets](https://leetcode.com/problems/subsets/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return all subsets (the power set) of an array of **distinct** integers. Any order; no duplicate subsets.

**Clarify:** Distinct elements? Should the empty set be included? (Yes.)

## 2. Brute Force

Generate all `2^n` bitmasks and build a subset for each mask.

## 3. Brute-Force Complexity

Time `O(n · 2^n)`, Space `O(n)` + output.

## 4. Bottleneck

Not a real bottleneck; the output itself is `2^n`. The goal is a clear recursive structure that extends to harder backtracking problems.

## 5. Optimal Approach (and why it works)

For each element there are two choices: **include** or **exclude**. Recurse index by index; when `i == n`, record the current subset. After the include branch, pop the element (backtrack) and explore exclude.

**Complexity:** Time `O(n · 2^n)`, Space `O(n)` recursion + output.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(int i, vector<int>& nums, vector<int>& cur, vector<vector<int>>& res) {
    if (i == (int)nums.size()) { res.push_back(cur); return; }
    cur.push_back(nums[i]);                   // include nums[i]
    dfs(i + 1, nums, cur, res);
    cur.pop_back();                           // backtrack: exclude nums[i]
    dfs(i + 1, nums, cur, res);
}

vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> cur;
    dfs(0, nums, cur, res);
    return res;
}
```

## 7. Dry Run

`nums = [1, 2]`

- include 1 → include 2 → record `[1,2]`; exclude 2 → record `[1]`
- exclude 1 → include 2 → record `[2]`; exclude 2 → record `[]`
- Result `[[1,2],[1],[2],[]]`

## 8. Edge Cases

- Empty input → `[[]]`
- Single element → `[[], [x]]`
- Duplicates in input (Subsets II): sort and skip equal siblings
- Maximum `n` is small (≤ ~20) because of `2^n`

## Likely Follow-ups

- Subsets with duplicates? → Sort, skip `nums[i] == nums[i-1]` at the same level.
- Bitmask solution? → `for mask in [0, 2^n)`.
- Subsets with sum = K?
