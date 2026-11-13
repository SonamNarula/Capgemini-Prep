# Rotate Image

| | |
|---|---|
| **Section** | 3. Strings & Matrix |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Transpose + reverse rows |
| **Problem** | [Rotate Image](https://leetcode.com/problems/rotate-image/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Rotate an `n×n` matrix 90° **clockwise**, in place.

**Clarify:** Clockwise or anticlockwise? Must it be in place? (Yes.)

## 2. Brute Force

Allocate a new matrix and place `m[i][j]` at `res[j][n-1-i]`, then copy back.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(n²)`.

## 4. Bottleneck

The extra matrix violates the in-place constraint.

## 5. Optimal Approach (and why it works)

A clockwise rotation equals **transpose** (swap `m[i][j]` with `m[j][i]` for `j > i`) followed by **reversing every row**. (Anticlockwise: transpose, then reverse every column, or reverse rows first then transpose.)

**Complexity:** Time `O(n²)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void rotate(vector<vector<int>>& m) {
    int n = m.size();
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
            swap(m[i][j], m[j][i]);           // transpose
    for (auto& row : m)
        reverse(row.begin(), row.end());      // mirror each row
}
```

## 7. Dry Run

`[[1,2],[3,4]]`

- Transpose → `[[1,3],[2,4]]`
- Reverse rows → `[[3,1],[4,2]]` (clockwise rotation ✓)

3×3: `[[1,2,3],[4,5,6],[7,8,9]]` → transpose `[[1,4,7],[2,5,8],[3,6,9]]` → reverse rows `[[7,4,1],[8,5,2],[9,6,3]]`

## 8. Edge Cases

- `1×1` matrix
- `2×2` matrix
- Odd `n` (centre stays fixed)
- Only swap `j > i` in the transpose, otherwise you undo your own swaps

## Likely Follow-ups

- Anticlockwise? → Transpose then reverse columns (or reverse rows first).
- 180° rotation? → Reverse rows, then reverse each row.
- Non-square matrix? → Can't be in place; allocate `m×n → n×m`.
