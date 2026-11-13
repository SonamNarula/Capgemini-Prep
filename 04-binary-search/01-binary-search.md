# Binary Search

| | |
|---|---|
| **Section** | 4. Binary Search |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Standard binary search |
| **Problem** | [Binary Search](https://leetcode.com/problems/binary-search/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given a sorted array and a `target`, return its index or `-1`.

**Clarify:** Sorted ascending? Distinct values? Can the array be empty?

## 2. Brute Force

Linear scan.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(1)`.

## 4. Bottleneck

Ignoring the sorted property; each comparison can eliminate half the array.

## 5. Optimal Approach (and why it works)

Keep `[lo, hi]` as the live range. Compare `nums[mid]` with `target` and discard the half that cannot contain it. Compute `mid = lo + (hi - lo) / 2` to avoid overflow.

**Complexity:** Time `O(log n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int search(vector<int>& nums, int target) {
    int lo = 0, hi = (int)nums.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] == target) return mid;
        if (nums[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
```

## 7. Dry Run

`nums = [-1,0,3,5,9,12]`, `target = 9`

- `lo=0, hi=5, mid=2 (3)` < 9 → `lo=3`
- `lo=3, hi=5, mid=4 (9)` = target → return `4`

## 8. Edge Cases

- Empty array → `-1`
- Single element (hit and miss)
- Target smaller than first / larger than last
- Large indexes: `lo + (hi-lo)/2` avoids overflow
- Loop condition `<=` with `hi = mid - 1` must be used together

## Likely Follow-ups

- Duplicates, find first/last? → See 'First and Last Occurrence'.
- Recursive version? → Same logic, `O(log n)` stack.
