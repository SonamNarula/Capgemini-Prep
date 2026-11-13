# BFS traversal

| | |
|---|---|
| **Section** | 12. Graph Fundamentals |
| **Priority** | P2 |
| **Difficulty** | Foundational |
| **Key pattern** | Queue |
| **Problem** | BFS traversal *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Visit all vertices reachable from a start vertex **level by level** (nearest first). Also gives shortest path length in an unweighted graph.

**Clarify:** Connected graph or possibly disconnected? Need distances / parents?

## 2. Brute Force

No simpler alternative; the key is to avoid revisiting vertices (otherwise infinite loops on cycles).

## 3. Brute-Force Complexity

Without a visited array: may loop forever in cyclic graphs.

## 4. Bottleneck

Duplicate visits. Mark vertices as visited **when enqueued** so each vertex enters the queue once.

## 5. Optimal Approach (and why it works)

Queue + `visited[]`. Push the start (mark visited). Repeatedly pop `u`, process it, and push every unvisited neighbour (mark immediately). For disconnected graphs, repeat from every unvisited vertex.

**Complexity:** Time `O(V + E)`, Space `O(V)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> bfs(int start, const vector<vector<int>>& adj) {
    int n = adj.size();
    vector<bool> vis(n, false);
    vector<int> order;
    queue<int> q;
    vis[start] = true;                         // mark when pushing
    q.push(start);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : adj[u])
            if (!vis[v]) {
                vis[v] = true;
                q.push(v);
            }
    }
    return order;
}
// Shortest path in unweighted graph: store dist[v] = dist[u] + 1 when pushing v.
```

## 7. Dry Run

`0:{1,2}`, `1:{0,3}`, `2:{0,3}`, `3:{1,2}`, start `0`

- queue `[0]` → pop 0 → push `1,2`
- pop 1 → push `3`
- pop 2 → `3` already visited
- pop 3
- Order `0 1 2 3`

## 8. Edge Cases

- Single vertex
- Disconnected graph (BFS from one start only covers its component)
- Cycles → rely on `visited`
- Self-loop
- Marking visited at *pop* time can insert duplicates in the queue

## Likely Follow-ups

- Shortest path in unweighted graph? → Distances from BFS levels.
- Multi-source BFS (rotting oranges, 01-matrix)?
- BFS on grids: neighbours via direction arrays.
