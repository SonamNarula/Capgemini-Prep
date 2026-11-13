# Number of connected components

| | |
|---|---|
| **Section** | 12. Graph Fundamentals |
| **Priority** | P2 |
| **Difficulty** | Foundational |
| **Key pattern** | DFS / BFS |
| **Problem** | Number of connected components *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Count groups of vertices that are connected to each other in an undirected graph.

**Clarify:** Vertices `0..n-1`? Isolated vertices count as their own component?

## 2. Brute Force

Run a shortest-path/closure computation between all pairs (`O(V³)`) and group vertices.

## 3. Brute-Force Complexity

Time `O(V³)` with Floyd-Warshall.

## 4. Bottleneck

We don't need pairwise distances; we only need to know which vertices a traversal can reach.

## 5. Optimal Approach (and why it works)

Loop over all vertices. Each time you find an **unvisited** vertex, start a DFS/BFS from it (marking its whole component visited) and increment the counter.

**Complexity:** Time `O(V + E)`, Space `O(V)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(int u, const vector<vector<int>>& adj, vector<bool>& vis) {
    vis[u] = true;
    for (int v : adj[u])
        if (!vis[v]) dfs(v, adj, vis);
}

int countComponents(int n, const vector<vector<int>>& adj) {
    vector<bool> vis(n, false);
    int comps = 0;
    for (int i = 0; i < n; i++) {
        if (!vis[i]) {
            comps++;
            dfs(i, adj, vis);                  // swallow the whole component
        }
    }
    return comps;
}
// Alternative: Union-Find; components = n - (number of successful unions).
```

## 7. Dry Run

`n = 5`, edges `(0,1) (1,2) (3,4)`

- `i=0` unvisited → `comps=1`, DFS marks `0,1,2`
- `i=1,2` visited
- `i=3` unvisited → `comps=2`, DFS marks `3,4`
- Answer `2`

## 8. Edge Cases

- No edges → `n` components
- Fully connected → `1`
- `n = 0` → `0`
- Isolated vertices each count as one
- Duplicate edges / self-loops

## Likely Follow-ups

- Union-Find solution?
- Largest component size? → Return DFS size per component.
- Directed graph? → Strongly connected components (Kosaraju/Tarjan).
