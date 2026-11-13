# K Closest Points to Origin

| | |
|---|---|
| **Section** | 7. Heaps & Priority Queues |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Heap |
| **Problem** | [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the `k` points closest to the origin `(0,0)` by Euclidean distance. Any order is acceptable.

**Clarify:** Ties at the boundary? (Guaranteed unique answer.) Can I skip the square root?

## 2. Brute Force

Sort all points by distance and take the first `k`.

## 3. Brute-Force Complexity

Time `O(n log n)`, Space `O(1)`–`O(n)`.

## 4. Bottleneck

Full sort is more work than necessary for just `k` points. Also the square root is unnecessary since comparison needs only squared distance.

## 5. Optimal Approach (and why it works)

Keep a **max-heap of size `k`** keyed by squared distance `x² + y²`. Push each point; if size exceeds `k`, pop the farthest. What remains are the `k` closest. Use `long long` for squared distance to avoid overflow.

**Complexity:** Time `O(n log k)`, Space `O(k)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<vector<int>> kClosest(vector<vector<int>>& pts, int k) {
    priority_queue<pair<long long, int>> pq;      // (dist^2, index), max-heap
    for (int i = 0; i < (int)pts.size(); i++) {
        long long d = 1LL * pts[i][0] * pts[i][0] + 1LL * pts[i][1] * pts[i][1];
        pq.push({d, i});
        if ((int)pq.size() > k) pq.pop();         // drop the farthest
    }
    vector<vector<int>> res;
    while (!pq.empty()) {
        res.push_back(pts[pq.top().second]);
        pq.pop();
    }
    return res;
}
```

## 7. Dry Run

`points = [[1,3],[-2,2]]`, `k = 1`

- `(1,3)`: `d=10` → heap `{10}`
- `(-2,2)`: `d=8` → heap `{10,8}`, size 2 > 1 → pop `10`
- Result `[[-2,2]]`

## 8. Edge Cases

- `k == n` → all points
- Points at the origin `(0,0)`
- Negative coordinates
- Equal distances
- Large coordinates → `long long` squared distance

## Likely Follow-ups

- Faster average? → Quickselect / `nth_element` on distance, `O(n)` average.
- Why avoid `sqrt`? → Monotonic, saves time and floating-point error.
