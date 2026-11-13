# DFS traversal

| | |
|---|---|
| **Section** | 12. Graph Fundamentals |
| **Priority** | P2 |
| **Difficulty** | Foundational |
| **Key pattern** | Recursion |
| **Problem** | DFS traversal *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Visit vertices by going as deep as possible along each branch before backtracking.

**Clarify:** Recursive acceptable? Graph size (recursion depth risk at `1e5`)?

## 2. Brute Force

No simpler alternative; again `visited[]` prevents infinite loops.

## 3. Brute-Force Complexity

Without a visited array: infinite recursion on cycles.

## 4. Bottleneck

Revisiting vertices. A single `visited` array makes the traversal `O(V + E)`.

## 5. Optimal Approach (and why it works)

`dfs(u)`: mark visited, process, recurse into each unvisited neighbour. For disconnected graphs, call `dfs` from every unvisited vertex. For very deep graphs, use an explicit stack.

**Complexity:** Time `O(V + E)`, Space `O(V)` (recursion/stack).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(int u, const vector<vector<int>>& adj, vector<bool>& vis, vector<int>& order) {
    vis[u] = true;
    order.push_back(u);
    for (int v : adj[u])
        if (!vis[v]) dfs(v, adj, vis, order);
}

vector<int> dfsTraversal(int start, const vector<vector<int>>& adj) {
    vector<bool> vis(adj.size(), false);
    vector<int> order;
    dfs(start, adj, vis, order);
    return order;
}

// Iterative version (avoids stack overflow on deep graphs)
vector<int> dfsIterative(int start, const vector<vector<int>>& adj) {
    vector<bool> vis(adj.size(), false);
    vector<int> order;
    stack<int> st;
    st.push(start);
    while (!st.empty()) {
        int u = st.top(); st.pop();
        if (vis[u]) continue;
        vis[u] = true;
        order.push_back(u);
        for (int i = (int)adj[u].size() - 1; i >= 0; i--)   // reverse to match recursive order
            if (!vis[adj[u][i]]) st.push(adj[u][i]);
    }
    return order;
}
```

## 7. Dry Run

Same graph, start `0`

- visit 0 → neighbour 1 → neighbour 3 → neighbour 2 (via `3`'s adjacency)
- Order `0 1 3 2`

## 8. Edge Cases

- Single vertex
- Disconnected graph
- Cycles
- Deep chain graph (`1e5`) → recursion stack overflow, go iterative
- Directed vs undirected only changes how edges were stored

## Likely Follow-ups

- Difference from BFS? → Stack/recursion (depth-first) vs queue (level-order); DFS doesn't give shortest paths.
- Pre-order vs post-order timing in DFS (used in topological sort).
