# Trapping Rain Water

| | |
|---|---|
| **Section** | 2. Two Pointers & Sliding Window |
| **Priority** | P0 |
| **Difficulty** | Hard |
| **Key pattern** | Two pointers / leftMax and rightMax |
| **Problem** | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given bar heights, compute the total rainwater trapped between bars.

**Clarify:** Non-negative heights? Water can't escape the ends?

## 2. Brute Force

For each index, scan left for the max height and right for the max height; water at `i` is `min(leftMax, rightMax) - h[i]`.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

`leftMax`/`rightMax` are recomputed for every index. They can be precomputed (arrays, `O(n)` space), or tracked on the fly.

## 5. Optimal Approach (and why it works)

Water above bar `i` = `min(maxLeft, maxRight) - h[i]`. Use two pointers: whichever side currently has the smaller bar is bounded by its own side's max (the other side's max is at least as large), so its water can be finalised now. Update that side's max, add `max - h`, and move that pointer inward.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int trap(vector<int>& h) {
    int l = 0, r = (int)h.size() - 1;
    int lmax = 0, rmax = 0, water = 0;
    while (l < r) {
        if (h[l] < h[r]) {
            lmax = max(lmax, h[l]);
            water += lmax - h[l];
            l++;
        } else {
            rmax = max(rmax, h[r]);
            water += rmax - h[r];
            r--;
        }
    }
    return water;
}
```

## 7. Dry Run

`[3, 0, 2, 0, 4]`

- `l=0 (3) < r=4 (4)`: `lmax=3`, water `0`, `l=1`
- `h[1]=0 < 4`: water `+3` → 3, `l=2`
- `h[2]=2 < 4`: water `+1` → 4, `l=3`
- `h[3]=0 < 4`: water `+3` → 7, `l=4` → stop
- Answer `7`

## 8. Edge Cases

- Fewer than 3 bars → `0`
- Monotonic increasing or decreasing → `0`
- All bars equal → `0`
- Single tall wall with valleys either side
- Large `n`: result can exceed `int` in extreme cases, use `long long` if required

## Likely Follow-ups

- Prefix/suffix array version first? → `leftMax[]`, `rightMax[]`, `O(n)` space, then optimise.
- Monotonic stack version? → Also `O(n)`, computes water in horizontal layers.
