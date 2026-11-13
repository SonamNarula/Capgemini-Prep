# First and Last Occurrence in Sorted Array

| | |
|---|---|
| **Section** | 4. Binary Search |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Lower/upper bound |
| **Problem** | [First and Last Occurrence in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given a sorted array (duplicates allowed) and `x`, return the first and last index of `x`, or `[-1, -1]`.

**Clarify:** Sorted ascending? Expected `O(log n)`?

## 2. Brute Force

Linear scan, recording first and last matches.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(1)`.

## 4. Bottleneck

With duplicates a normal binary search lands on *some* occurrence. Two boundary searches find the extremes.

## 5. Optimal Approach (and why it works)

First occurrence = `lower_bound(x)` (first element `≥ x`). Last occurrence = `upper_bound(x) - 1` (one before first element `> x`). Verify that the lower bound actually holds `x`.

**Complexity:** Time `O(log n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
// STL version
vector<int> searchRange(vector<int>& a, int x) {
    auto lo = lower_bound(a.begin(), a.end(), x);
    if (lo == a.end() || *lo != x) return {-1, -1};
    auto hi = upper_bound(a.begin(), a.end(), x);
    return {int(lo - a.begin()), int(hi - a.begin()) - 1};
}

// Manual version (write this if the interviewer bans STL)
int lowerBound(const vector<int>& a, int x) {         // first index with a[i] >= x
    int lo = 0, hi = a.size();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (a[mid] < x) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
// last occurrence = lowerBound(a, x + 1) - 1  (guard against x == INT_MAX)
```

## 7. Dry Run

`a = [5,7,7,8,8,10]`, `x = 8`

- `lower_bound(8)` → index `3`
- `upper_bound(8)` → index `5` → last = `5 - 1 = 4`
- Result `[3, 4]`

`x = 6` → `lower_bound` lands on `7` (≠ 6) → `[-1, -1]`

## 8. Edge Cases

- Element absent → `[-1,-1]`
- Single occurrence → first == last
- All elements equal to `x`
- `x` smaller than all / larger than all
- Empty array

## Likely Follow-ups

- Count of occurrences? → `upper_bound - lower_bound`.
- Why check `*lo != x`? → `lower_bound` returns the insertion point, which may hold a different value.
