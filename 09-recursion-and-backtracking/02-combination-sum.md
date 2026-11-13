# Combination Sum

| | |
|---|---|
| **Section** | 9. Recursion & Backtracking |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Backtracking |
| **Problem** | [Combination Sum](https://leetcode.com/problems/combination-sum/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given **distinct** positive integers `candidates` and a `target`, return all unique combinations that sum to `target`. Each number can be used **unlimited** times.

**Clarify:** Combinations (order doesn't matter), so `[2,3]` and `[3,2]` are the same.

## 2. Brute Force

Generate all multisets of candidates up to `target/min` size and filter by sum.

## 3. Brute-Force Complexity

Exponential with heavy waste.

## 4. Bottleneck

We need to prune early (stop when the remainder goes negative) and avoid permutations of the same combination.

## 5. Optimal Approach (and why it works)

DFS with a `start` index. At each step, try candidates from `start` onward (so combinations are generated in non-decreasing index order, which avoids duplicates). Recurse with the **same** index `i` to allow reuse. Sort first and `break` when `c[i] > remain` to prune. Record when `remain == 0`.

**Complexity:** Time roughly `O(N^(T/M))` (`T` target, `M` smallest candidate), Space `O(T/M)` recursion depth.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(int start, int remain, vector<int>& c, vector<int>& cur, vector<vector<int>>& res) {
    if (remain == 0) { res.push_back(cur); return; }
    for (int i = start; i < (int)c.size(); i++) {
        if (c[i] > remain) break;                    // sorted: nothing further fits
        cur.push_back(c[i]);
        dfs(i, remain - c[i], c, cur, res);          // i (not i+1): reuse allowed
        cur.pop_back();
    }
}

vector<vector<int>> combinationSum(vector<int>& c, int target) {
    sort(c.begin(), c.end());
    vector<vector<int>> res;
    vector<int> cur;
    dfs(0, target, c, cur, res);
    return res;
}
```

## 7. Dry Run

`c = [2,3,6,7]`, `target = 7`

- `[2]` → `[2,2]` → `[2,2,2]` (remain 1, `2 > 1` → break) → back, `[2,2,3]` → remain 0 ✓
- `[2,3]` → `[2,3,3]` too big → prune
- `[3]` → ... no; `[6]` → no; `[7]` → remain 0 ✓
- Result `[[2,2,3],[7]]`

## 8. Edge Cases

- No combination exists → `[]`
- Target smaller than every candidate
- Single candidate `[2]`, target `4` → `[[2,2]]`
- Candidate equal to target
- Duplicates in candidates (Combination Sum II): skip equal siblings and use `i+1`

## Likely Follow-ups

- Each number only once (LC 40)? → Recurse with `i+1`, skip duplicates.
- Why pass `i` rather than `i+1`? → Allows repeated use of the same number.
- Why sort? → Enables the `break` pruning.
