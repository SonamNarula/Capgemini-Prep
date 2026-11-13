# Cycle detection in an undirected graph

| | |
|---|---|
| **Section** | 12. Graph Fundamentals |
| **Priority** | P2 |
| **Difficulty** | Foundational |
| **Key pattern** | DFS / BFS |
| **Problem** | Cycle detection in an undirected graph *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Determine whether an undirected graph contains a cycle.

**Clarify:** Can the graph be disconnected? Are self-loops or parallel edges possible?

## 2. Brute Force

Remove each edge and check whether connectivity changes (bridges-style); heavy, roughly `O(E · (V + E))`.

## 3. Brute-Force Complexity

Time `O(E · (V + E))`.

## 4. Bottleneck

A single traversal can detect a cycle: reaching an already-visited vertex that is *not* the vertex we just came from means a back edge.

## 5. Optimal Approach (and why it works)

DFS carrying the `parent`. For each neighbour `v` of `u`: if `v` is unvisited, recurse with `parent = u`; if `v` is visited and `v != parent`, a cycle exists. Run from every unvisited vertex to cover disconnected graphs.

**Complexity:** Time `O(V + E)`, Space `O(V)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
bool dfs(int u, int parent, const vector<vector<int>>& adj, vector<bool>& vis) {
    vis[u] = true;
    for (int v : adj[u]) {
        if (!vis[v]) {
            if (dfs(v, u, adj, vis)) return true;
        } else if (v != parent) {
            return true;                        // visited non-parent => back edge
        }
    }
    return false;
}

bool hasCycle(int n, const vector<vector<int>>& adj) {
    vector<bool> vis(n, false);
    for (int i = 0; i < n; i++)
        if (!vis[i] && dfs(i, -1, adj, vis)) return true;
    return false;
}
// Alternatives: BFS storing parent, or Union-Find (if find(u) == find(v) before union => cycle).
```

## 7. Dry Run

Triangle `0-1, 1-2, 2-0`

- `dfs(0, -1)`: visit `1` (parent 0)
- `dfs(1, 0)`: visit `2` (parent 1)
- `dfs(2, 1)`: neighbour `0` is visited and `0 != parent 1` → cycle → `true`

Path `0-1-2`: neighbours only point back to parents → `false`

## 8. Edge Cases

- Tree (no cycle) → `false`
- Disconnected graph with a cycle in one component → still `true` (loop over all vertices)
- Self-loop `u-u` → cycle
- Parallel edges `u-v` twice → a cycle, but the simple parent check misses it; track the **edge id** instead
- Single vertex / empty graph → `false`

## Likely Follow-ups

- Directed graph cycle detection? → DFS with recursion-stack colouring or Kahn's algorithm.
- Why can't we use the same parent trick for directed graphs?
- Union-Find version complexity? → `O(E α(V))`.
