# Spiral Matrix

| | |
|---|---|
| **Section** | 3. Strings & Matrix |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Four boundaries |
| **Problem** | [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return all elements of an `m×n` matrix in clockwise spiral order.

**Clarify:** Rectangular (not just square)? Empty matrix?

## 2. Brute Force

Simulate with a `visited` matrix and a direction vector, turning when hitting a boundary or visited cell.

## 3. Brute-Force Complexity

Time `O(m·n)`, Space `O(m·n)`.

## 4. Bottleneck

Visited bookkeeping is unnecessary. Four shrinking boundaries encode the same information.

## 5. Optimal Approach (and why it works)

Maintain `top, bottom, left, right`. Traverse top row (left→right) then `top++`; right column (top→bottom) then `right--`; bottom row (right→left) then `bottom--` **if** `top ≤ bottom`; left column (bottom→top) then `left++` **if** `left ≤ right`. Those two guards prevent double-visiting in single-row/column leftovers.

**Complexity:** Time `O(m·n)`, Space `O(1)` extra.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> spiralOrder(vector<vector<int>>& m) {
    vector<int> res;
    if (m.empty() || m[0].empty()) return res;
    int top = 0, bot = (int)m.size() - 1;
    int left = 0, right = (int)m[0].size() - 1;
    while (top <= bot && left <= right) {
        for (int j = left; j <= right; j++) res.push_back(m[top][j]);
        top++;
        for (int i = top; i <= bot; i++) res.push_back(m[i][right]);
        right--;
        if (top <= bot) {
            for (int j = right; j >= left; j--) res.push_back(m[bot][j]);
            bot--;
        }
        if (left <= right) {
            for (int i = bot; i >= top; i--) res.push_back(m[i][left]);
            left++;
        }
    }
    return res;
}
```

## 7. Dry Run

`[[1,2,3],[4,5,6],[7,8,9]]`

- Top row: `1 2 3`, `top=1`
- Right col: `6 9`, `right=1`
- Bottom row: `8 7`, `bot=1`
- Left col: `4`, `left=1`
- Loop again: top row `5`, then everything collapses
- Result `1 2 3 6 9 8 7 4 5`

## 8. Edge Cases

- Single row `[[1,2,3]]` (the `top <= bot` guard stops duplicates)
- Single column `[[1],[2],[3]]` (the `left <= right` guard)
- Empty matrix
- `1×1`
- Non-square e.g. `3×4`

## Likely Follow-ups

- Generate a spiral matrix from `n` (LC 59)? → Same boundaries, fill instead of read.
- Counter-clockwise spiral? → Change traversal order.
