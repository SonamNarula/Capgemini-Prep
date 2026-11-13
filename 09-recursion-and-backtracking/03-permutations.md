# Permutations

| | |
|---|---|
| **Section** | 9. Recursion & Backtracking |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Visited array |
| **Problem** | [Permutations](https://leetcode.com/problems/permutations/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return all permutations of an array of **distinct** integers.

**Clarify:** Distinct values? Output order flexible?

## 2. Brute Force

Generate all `n^n` sequences and filter those that use each element exactly once.

## 3. Brute-Force Complexity

Time `O(n^n)`.

## 4. Bottleneck

We can build only valid sequences by marking which elements are already used.

## 5. Optimal Approach (and why it works)

Build the permutation position by position. Maintain a `used[]` array. For each position, try every unused element: mark used, recurse, then unmark (backtrack). When `cur.size() == n`, record.

**Complexity:** Time `O(n · n!)`, Space `O(n)` recursion + output.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(vector<int>& nums, vector<bool>& used, vector<int>& cur, vector<vector<int>>& res) {
    if (cur.size() == nums.size()) { res.push_back(cur); return; }
    for (int i = 0; i < (int)nums.size(); i++) {
        if (used[i]) continue;
        used[i] = true;
        cur.push_back(nums[i]);
        dfs(nums, used, cur, res);
        cur.pop_back();                            // restore state
        used[i] = false;
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> cur;
    vector<bool> used(nums.size(), false);
    dfs(nums, used, cur, res);
    return res;
}
// Alternative: std::sort + do { ... } while (next_permutation(...)) in O(n * n!)
```

## 7. Dry Run

`[1, 2, 3]`

- pick 1 → pick 2 → pick 3 → `[1,2,3]`; back, pick 3 → pick 2 → `[1,3,2]`
- pick 2 → `[2,1,3]`, `[2,3,1]`
- pick 3 → `[3,1,2]`, `[3,2,1]`
- `3! = 6` permutations

## 8. Edge Cases

- Single element → `[[x]]`
- Empty input → `[[]]`
- Duplicates (Permutations II): sort and skip `i` if `nums[i]==nums[i-1] && !used[i-1]`
- Negative numbers fine

## Likely Follow-ups

- Swap-based in-place variant?
- Next permutation (LC 31)? → Different, `O(n)` algorithm.
- Permutations with duplicates?
