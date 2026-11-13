# Graph representation

| | |
|---|---|
| **Section** | 12. Graph Fundamentals |
| **Priority** | P2 |
| **Difficulty** | Foundational |
| **Key pattern** | Adjacency list / matrix |
| **Problem** | Graph representation *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Store a graph of `n` vertices and `m` edges so algorithms can query neighbours efficiently. Know directed vs undirected and weighted vs unweighted.

**Clarify:** Dense or sparse? Vertices labelled `0..n-1` or `1..n`? Weights needed?

## 2. Brute Force

Keep the raw edge list and scan all edges to find the neighbours of a vertex.

## 3. Brute-Force Complexity

Neighbour lookup `O(m)`.

## 4. Bottleneck

Algorithms like BFS/DFS ask for neighbours repeatedly; scanning all edges each time is too slow.

## 5. Optimal Approach (and why it works)

**Adjacency list** (`vector<vector<int>>`, or `vector<vector<pair<int,int>>>` for weights): space `O(n + m)`, iterating neighbours costs `deg(v)`. Best for sparse graphs. **Adjacency matrix** (`vector<vector<int>>`, `n × n`): space `O(n²)`, edge check in `O(1)`. Best for dense graphs or small `n`.

**Complexity:** List: Space `O(n + m)`, edge check `O(deg)`. Matrix: Space `O(n²)`, edge check `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
// Adjacency list (unweighted)
vector<vector<int>> buildAdjList(int n, const vector<pair<int,int>>& edges, bool directed = false) {
    vector<vector<int>> adj(n);
    for (auto& [u, v] : edges) {
        adj[u].push_back(v);
        if (!directed) adj[v].push_back(u);       // undirected: add both ways
    }
    return adj;
}

// Weighted adjacency list: adj[u] holds {v, weight}
vector<vector<pair<int,int>>> buildWeighted(int n, const vector<array<int,3>>& edges) {
    vector<vector<pair<int,int>>> adj(n);
    for (auto& e : edges) {
        adj[e[0]].push_back({e[1], e[2]});
        adj[e[1]].push_back({e[0], e[2]});
    }
    return adj;
}

// Adjacency matrix
vector<vector<int>> buildMatrix(int n, const vector<pair<int,int>>& edges, bool directed = false) {
    vector<vector<int>> g(n, vector<int>(n, 0));
    for (auto& [u, v] : edges) {
        g[u][v] = 1;
        if (!directed) g[v][u] = 1;
    }
    return g;
}
```

## 7. Dry Run

`n = 4`, edges `(0,1) (0,2) (1,3) (2,3)` (undirected)

- `adj[0] = {1,2}`
- `adj[1] = {0,3}`
- `adj[2] = {0,3}`
- `adj[3] = {1,2}`
- Matrix has `1` at `[0][1], [1][0], [0][2], [2][0], [1][3], [3][1], [2][3], [3][2]`

## 8. Edge Cases

- Vertices labelled `1..n` → allocate `n + 1`
- Disconnected graph / isolated vertices
- Self-loops and parallel edges
- Undirected edges must be added **both** ways
- `n` large (`1e5`) → matrix impossible, use a list

## Likely Follow-ups

- When would you prefer a matrix? → Dense graphs, Floyd-Warshall, frequent edge-existence checks.
- Memory for `n = 1e5`, `m = 2e5`? → List is tiny; matrix would be `1e10` cells.
