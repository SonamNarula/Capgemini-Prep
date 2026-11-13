# Last Stone Weight

| | |
|---|---|
| **Section** | 7. Heaps & Priority Queues |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Max-heap |
| **Problem** | [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Repeatedly smash the two heaviest stones: equal → both vanish; otherwise the lighter vanishes and the heavier becomes `y - x`. Return the weight of the last stone (or `0`).

**Clarify:** Empty input?

## 2. Brute Force

Sort the array every round, take the last two, update, repeat.

## 3. Brute-Force Complexity

Time `O(n² log n)`, Space `O(1)`.

## 4. Bottleneck

Re-sorting for each smash. A max-heap gives the two largest in `O(log n)`.

## 5. Optimal Approach (and why it works)

Push all stones into a max-heap (`priority_queue<int>`). While at least two remain: pop the two largest `a ≥ b`; if `a != b` push `a - b`. Return the remaining stone or `0`.

**Complexity:** Time `O(n log n)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int lastStoneWeight(vector<int>& stones) {
    priority_queue<int> pq(stones.begin(), stones.end());   // max-heap
    while (pq.size() > 1) {
        int a = pq.top(); pq.pop();
        int b = pq.top(); pq.pop();
        if (a != b) pq.push(a - b);
    }
    return pq.empty() ? 0 : pq.top();
}
```

## 7. Dry Run

`[2,7,4,1,8,1]`

- `8,7 → 1`: `[2,4,1,1,1]`
- `4,2 → 2`: `[2,1,1,1]`
- `2,1 → 1`: `[1,1,1]`
- `1,1 → 0`: `[1]`
- Answer `1`

## 8. Edge Cases

- Single stone → itself
- Two equal stones → `0`
- All equal even count → `0`
- Empty array → `0`
- Stones of weight 1

## Likely Follow-ups

- Why a heap and not sorting once? → The new stone must be re-inserted in order each round.
- K-th largest or merge-style problems use the same container.
