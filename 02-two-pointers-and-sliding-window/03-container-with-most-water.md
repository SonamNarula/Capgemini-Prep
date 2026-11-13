# Container With Most Water

| | |
|---|---|
| **Section** | 2. Two Pointers & Sliding Window |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Greedy two pointers |
| **Problem** | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given heights `h[i]` as vertical lines, choose two lines that, with the x-axis, hold the most water. Area = `(j - i) · min(h[i], h[j])`.

**Clarify:** Lines can't be slanted? Heights non-negative?

## 2. Brute Force

Check every pair `(i, j)` and take the max area.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

Most pairs can be proven useless without evaluation.

## 5. Optimal Approach (and why it works)

Start with the widest container (`l=0`, `r=n-1`). Area is limited by the **shorter** line. Moving the taller line inward can only shrink width without raising the limiting height, so it can never help. Move the shorter side inward and hope for a taller line.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int maxArea(vector<int>& h) {
    int l = 0, r = (int)h.size() - 1, best = 0;
    while (l < r) {
        best = max(best, (r - l) * min(h[l], h[r]));
        if (h[l] < h[r]) l++;
        else r--;
    }
    return best;
}
```

## 7. Dry Run

`[1,8,6,2,5,4,8,3,7]`

- `l=0,r=8`: `8·min(1,7)=8`; `h[l]` smaller → `l=1`
- `l=1,r=8`: `7·min(8,7)=49`; `h[r]` smaller → `r=7`
- `l=1,r=7`: `6·3=18` → `r=6`
- `l=1,r=6`: `5·8=40` → ... best stays `49`

## 8. Edge Cases

- Two elements → `min(h0,h1)`
- All heights equal
- Zeros in heights
- Strictly increasing or decreasing array
- `n = 2` is the minimum valid input

## Likely Follow-ups

- Prove why skipping the taller line is safe. → See the greedy argument above.
- Difference from Trapping Rain Water? → There, bars *between* matter; here only the two walls.
