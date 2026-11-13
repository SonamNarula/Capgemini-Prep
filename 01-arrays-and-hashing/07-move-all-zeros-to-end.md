# Move All Zeros to End

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Two pointers, in-place |
| **Problem** | Move All Zeros to End *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Move all `0`s to the end while keeping the relative order of non-zero elements. Must be in place.

**Clarify:** Is minimising writes/swaps important? Is order of non-zeros required? (Yes.)

## 2. Brute Force

Copy non-zero elements into a new array, then fill the rest with zeros, then copy back.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

The extra array is unnecessary; the task asks for in-place.

## 5. Optimal Approach (and why it works)

Keep a write pointer `j` marking where the next non-zero belongs. Scan with `i`; when `nums[i] != 0`, swap it with `nums[j]` and advance `j`. Everything before `j` is non-zero in original order; everything between `j` and `i` is zero.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void moveZeroes(vector<int>& nums) {
    int j = 0;                                // next slot for a non-zero
    for (int i = 0; i < (int)nums.size(); i++) {
        if (nums[i] != 0) {
            swap(nums[i], nums[j]);
            j++;
        }
    }
}
```

## 7. Dry Run

`[0, 1, 0, 3, 12]`

- `i=0`: zero, skip
- `i=1`: `1` → swap with `j=0` → `[1,0,0,3,12]`, `j=1`
- `i=2`: zero, skip
- `i=3`: `3` → swap with `j=1` → `[1,3,0,0,12]`, `j=2`
- `i=4`: `12` → swap with `j=2` → `[1,3,12,0,0]`

## 8. Edge Cases

- All zeros `[0,0,0]`
- No zeros `[1,2,3]` (swaps with itself; harmless)
- Single element
- Already arranged `[1,2,0,0]`
- Empty array

## Likely Follow-ups

- Minimise writes? → Only swap when `i != j`, or copy non-zeros then fill zeros.
- Move zeros to the *front* instead? → Mirror: iterate from the right.
