# Find Equilibrium / Pivot Index

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Total sum minus left prefix |
| **Problem** | [Find Equilibrium / Pivot Index](https://leetcode.com/problems/find-pivot-index/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Find the leftmost index where the sum of elements to its left equals the sum of elements to its right (the element itself excluded). Return `-1` if none.

**Clarify:** Does an empty side count as sum `0`? (Yes.) Leftmost index if several?

## 2. Brute Force

For each index, compute left sum and right sum with two loops and compare.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

Left and right sums are recomputed from scratch for every index.

## 5. Optimal Approach (and why it works)

Compute `total` once. Maintain `left` as a running prefix. At index `i` the right sum is `total - left - nums[i]`. If `left == right`, return `i`; otherwise add `nums[i]` to `left`.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int pivotIndex(vector<int>& nums) {
    long long total = accumulate(nums.begin(), nums.end(), 0LL);
    long long left = 0;
    for (int i = 0; i < (int)nums.size(); i++) {
        long long right = total - left - nums[i];
        if (left == right) return i;
        left += nums[i];
    }
    return -1;
}
```

## 7. Dry Run

`[1, 7, 3, 6, 5, 6]`, total `28`

- `i=0`: left 0, right 27 → no
- `i=1`: left 1, right 20 → no
- `i=2`: left 8, right 17 → no
- `i=3`: left 11, right `28-11-6=11` → match → return `3`

## 8. Edge Cases

- Pivot at index `0` (left sum `0`): `[2,1,-1]` → `0`
- Pivot at last index `[-1,-1,0,1,1,0]`
- No pivot `[1,2,3]` → `-1`
- Single element → `0`
- Negatives and zeros

## Likely Follow-ups

- Return all pivots? → Don't return early; collect.
- Why `long long`? → Prevent overflow of `total` for large inputs.
