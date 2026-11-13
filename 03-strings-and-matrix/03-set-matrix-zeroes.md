# Set Matrix Zeroes

| | |
|---|---|
| **Section** | 3. Strings & Matrix |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | In-place markers |
| **Problem** | [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

If any cell is `0`, set its entire row and column to `0`, in place.

**Clarify:** Is `O(1)` extra space required? Can the matrix contain negatives?

## 2. Brute Force

Copy the matrix; for every `0` in the copy, zero its row and column in the original.

## 3. Brute-Force Complexity

Time `O(m·n·(m+n))` worst case, Space `O(m·n)`.

## 4. Bottleneck

Zeroing while scanning corrupts the data (new zeros look like original zeros). We need to *record* first and *apply* later, without extra memory.

## 5. Optimal Approach (and why it works)

Use the first row and first column as marker storage. First remember whether row 0 / col 0 *originally* contained a zero. Then for each inner cell `(i,j)` that is `0`, set `m[i][0] = m[0][j] = 0`. Apply zeros to the inner cells using the markers, and finally zero row 0 / col 0 if their flags were set.

**Complexity:** Time `O(m·n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void setZeroes(vector<vector<int>>& m) {
    int R = m.size(), C = m[0].size();
    bool firstRow = false, firstCol = false;
    for (int j = 0; j < C; j++) if (m[0][j] == 0) firstRow = true;
    for (int i = 0; i < R; i++) if (m[i][0] == 0) firstCol = true;

    for (int i = 1; i < R; i++)                // record markers
        for (int j = 1; j < C; j++)
            if (m[i][j] == 0) { m[i][0] = 0; m[0][j] = 0; }

    for (int i = 1; i < R; i++)                // apply to inner cells
        for (int j = 1; j < C; j++)
            if (m[i][0] == 0 || m[0][j] == 0) m[i][j] = 0;

    if (firstRow) for (int j = 0; j < C; j++) m[0][j] = 0;
    if (firstCol) for (int i = 0; i < R; i++) m[i][0] = 0;
}
```

## 7. Dry Run

`[[1,1,1],[1,0,1],[1,1,1]]`

- Flags: `firstRow=false`, `firstCol=false`
- Cell `(1,1)=0` → mark `m[1][0]=0`, `m[0][1]=0`
- Apply: `(1,2)` row marker 0 → 0; `(2,1)` col marker 0 → 0
- Result `[[1,0,1],[0,0,0],[1,0,1]]`

## 8. Edge Cases

- Zero in first row or first column only (the flags protect these)
- Zero at `(0,0)`
- No zeros → unchanged
- Single row / single column
- All zeros

## Likely Follow-ups

- `O(m+n)` space version first? → Two boolean arrays for rows and columns.
- Why must the first row/column be handled *last*? → They hold the markers until then.
