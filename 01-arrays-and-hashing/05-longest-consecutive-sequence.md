# Longest Consecutive Sequence

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | `unordered_set`, sequence-start check |
| **Problem** | [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Find the length of the longest run of consecutive integers (like `1,2,3,4`) present in an unsorted array, in `O(n)`.

**Clarify:** Duplicates allowed? Order irrelevant? Empty array → `0`?

## 2. Brute Force

Sort the array, then scan counting runs (skipping duplicates). Or for each number, repeatedly search the array for `x+1`, `x+2`...

## 3. Brute-Force Complexity

Sort: Time `O(n log n)`, Space `O(1)`–`O(n)`. Repeated search: `O(n²)` or worse.

## 4. Bottleneck

Sorting costs `O(n log n)`, which violates the required `O(n)`. We need `O(1)` membership checks without ordering.

## 5. Optimal Approach (and why it works)

Put all numbers in an `unordered_set`. Only start counting from a number `x` that is the **start of a sequence** (`x-1` is not in the set). From there extend `x+1, x+2, …`. Each number is visited at most twice in total (once as a candidate start, once inside a chain), so the work is linear.

**Complexity:** Time `O(n)` average, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int longestConsecutive(vector<int>& nums) {
    unordered_set<int> st(nums.begin(), nums.end());
    int best = 0;
    for (int x : st) {
        if (st.count(x - 1)) continue;       // not a sequence start
        int cur = x, len = 1;
        while (st.count(cur + 1)) { cur++; len++; }
        best = max(best, len);
    }
    return best;
}
```

## 7. Dry Run

`nums = [100, 4, 200, 1, 3, 2]`

- `100`: `99` absent → start; `101` absent → len 1
- `4`: `3` present → skip
- `200`: start, len 1
- `1`: `0` absent → start; `2`,`3`,`4` present → len 4
- Answer `4`

## 8. Edge Cases

- Empty array → `0`
- Duplicates `[1,2,0,1]` → `3` (set removes them)
- Negative numbers `[-1,-2,-3]` → `3`
- All elements identical → `1`
- Single element → `1`

## Likely Follow-ups

- Why is the `x-1` check essential? → Without it, each chain is re-walked from every member → `O(n²)`.
- Union-Find alternative? → Yes, merge `x` with `x±1`; track component size.
