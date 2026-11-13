# Search in Rotated Sorted Array

| | |
|---|---|
| **Section** | 4. Binary Search |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Identify sorted half |
| **Problem** | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

A sorted array of **distinct** values was rotated at an unknown pivot (`[4,5,6,7,0,1,2]`). Find `target`'s index in `O(log n)`.

**Clarify:** Distinct values? (Duplicates degrade to `O(n)` worst case.)

## 2. Brute Force

Linear scan.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(1)`.

## 4. Bottleneck

Plain binary search fails because the whole array isn't sorted, but at least one half around `mid` always is.

## 5. Optimal Approach (and why it works)

At each step one half `[lo..mid]` or `[mid..hi]` is sorted. If `nums[lo] <= nums[mid]`, the left half is sorted: if `target` lies in `[nums[lo], nums[mid])` search left, else right. Otherwise the right half is sorted: apply the mirrored check.

**Complexity:** Time `O(log n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int search(vector<int>& nums, int target) {
    int lo = 0, hi = (int)nums.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) return mid;
        if (nums[lo] <= nums[mid]) {                       // left half sorted
            if (nums[lo] <= target && target < nums[mid]) hi = mid - 1;
            else lo = mid + 1;
        } else {                                           // right half sorted
            if (nums[mid] < target && target <= nums[hi]) lo = mid + 1;
            else hi = mid - 1;
        }
    }
    return -1;
}
```

## 7. Dry Run

`[4,5,6,7,0,1,2]`, `target = 0`

- `lo=0,hi=6,mid=3 (7)`: left sorted (`4 ≤ 7`), `0` not in `[4,7)` → `lo=4`
- `lo=4,hi=6,mid=5 (1)`: left sorted (`0 ≤ 1`), `0 ∈ [0,1)` → `hi=4`
- `mid=4 (0)` → found, return `4`

## 8. Edge Cases

- Not rotated (already sorted)
- Single element
- Two elements `[3,1]`, target `1` (the `<=` in `nums[lo] <= nums[mid]` matters)
- Target absent
- Rotation by `n-1`

## Likely Follow-ups

- Duplicates allowed (LC 81)? → If `nums[lo]==nums[mid]==nums[hi]`, shrink both ends: worst `O(n)`.
- Find the pivot first then binary search? → Equivalent two-step approach.
