# Number of Islands

| | |
|---|---|
| **Section** | 12. Graph Fundamentals |
| **Priority** | P2 |
| **Difficulty** | Medium |
| **Key pattern** | Grid DFS / BFS |
| **Problem** | [Number of Islands](https://leetcode.com/problems/number-of-islands/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

In a grid of `'1'` (land) and `'0'` (water), count the islands. An island is a group of land cells connected **4-directionally** (up/down/left/right).

**Clarify:** Are diagonals connected? (No.) May I modify the grid? Grid size?

## 2. Brute Force

For each land cell, do a search to find all connected cells, but without marking, the same island is counted repeatedly.

## 3. Brute-Force Complexity

Without visited tracking: counts duplicates / infinite loops.

## 4. Bottleneck

We need to mark cells so each island is counted exactly once. Marking can reuse the grid itself (`'1' → '0'`) to avoid extra memory.

## 5. Optimal Approach (and why it works)

Scan every cell. When you find `'1'`: increment the island count and flood-fill (DFS/BFS) all connected land, turning it to `'0'` (sinking the island). Continue scanning.

**Complexity:** Time `O(R · C)`, Space `O(R · C)` worst-case recursion (or `O(min(R, C))` with BFS).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(vector<vector<char>>& g, int i, int j) {
    int R = g.size(), C = g[0].size();
    if (i < 0 || j < 0 || i >= R || j >= C || g[i][j] != '1') return;
    g[i][j] = '0';                              // mark visited by sinking the cell
    dfs(g, i + 1, j);
    dfs(g, i - 1, j);
    dfs(g, i, j + 1);
    dfs(g, i, j - 1);
}

int numIslands(vector<vector<char>>& grid) {
    if (grid.empty()) return 0;
    int R = grid.size(), C = grid[0].size(), count = 0;
    for (int i = 0; i < R; i++)
        for (int j = 0; j < C; j++)
            if (grid[i][j] == '1') {
                count++;
                dfs(grid, i, j);
            }
    return count;
}
// For very large grids, use BFS with a queue to avoid deep recursion.
```

## 7. Dry Run

```
1 1 0 0
1 0 0 1
0 0 1 1
```

- `(0,0)` is `1` → `count=1`, DFS sinks `(0,0),(0,1),(1,0)`
- `(1,3)` is `1` → `count=2`, DFS sinks `(1,3),(2,3),(2,2)`
- Answer `2`

## 8. Edge Cases

- Empty grid → `0`
- All water → `0`
- All land → `1`
- Single cell `"1"`
- Diagonal-only neighbours are **not** connected
- Modifying the input; copy the grid or use a `visited` matrix if not allowed

## Likely Follow-ups

- Max area of island (LC 695)? → DFS returns the area.
- Count distinct island shapes?
- Union-Find solution for dynamic land additions (LC 305).
