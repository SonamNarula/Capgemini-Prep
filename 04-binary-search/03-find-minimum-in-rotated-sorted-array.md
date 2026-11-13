# Find Minimum in Rotated Sorted Array

| | |
|---|---|
| **Section** | 4. Binary Search |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Compare `mid` with `high` |
| **Problem** | [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the minimum element of a rotated sorted array of distinct values in `O(log n)`.

**Clarify:** Distinct values? Array may be unrotated?

## 2. Brute Force

Linear scan for the minimum.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(1)`.

## 4. Bottleneck

The minimum sits at the rotation point; the sorted structure lets us eliminate half each time.

## 5. Optimal Approach (and why it works)

Compare `nums[mid]` with `nums[hi]`. If `nums[mid] > nums[hi]`, the minimum is in the right part (`lo = mid + 1`). Otherwise the minimum is at `mid` or to its left (`hi = mid`). Loop while `lo < hi`; the answer is `nums[lo]`.

**Complexity:** Time `O(log n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int findMin(vector<int>& nums) {
    int lo = 0, hi = (int)nums.size() - 1;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (nums[mid] > nums[hi]) lo = mid + 1;   // min is to the right of mid
        else hi = mid;                            // mid could be the min
    }
    return nums[lo];
}
```

## 7. Dry Run

`[3,4,5,1,2]`

- `lo=0,hi=4,mid=2 (5)` > `nums[4]=2` → `lo=3`
- `lo=3,hi=4,mid=3 (1)` > 2? no → `hi=3`
- `lo==hi=3` → answer `1`

## 8. Edge Cases

- Not rotated `[1,2,3]` → first element
- Single element
- Two elements `[2,1]`
- Rotation so the min is at the last index
- With duplicates (LC 154): if `nums[mid]==nums[hi]` do `hi--`

## Likely Follow-ups

- Why compare with `hi` and not `lo`? → Comparing with `lo` is ambiguous for unrotated arrays.
- How many times was the array rotated? → The index of the minimum.
