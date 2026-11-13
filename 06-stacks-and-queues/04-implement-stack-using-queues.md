# Implement Stack Using Queues

| | |
|---|---|
| **Section** | 6. Stacks & Queues |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Queue |
| **Problem** | [Implement Stack Using Queues](https://leetcode.com/problems/implement-stack-using-queues/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Implement `push`, `pop`, `top`, `empty` of a LIFO stack using only standard queue operations (`push`, `front`, `pop`, `size`, `empty`).

**Clarify:** One queue or two allowed? Which operation should be cheap?

## 2. Brute Force

Two queues: on `push`, move everything to a helper queue, add the new element, move back.

## 3. Brute-Force Complexity

`push` `O(n)`, others `O(1)`; extra queue memory.

## 4. Bottleneck

The second queue isn't necessary; rotating a single queue achieves the same ordering.

## 5. Optimal Approach (and why it works)

Single queue. After pushing `x`, rotate the queue `size-1` times (move the front to the back) so `x` becomes the front. Then `pop`/`top` are `O(1)` and mimic LIFO.

**Complexity:** `push` `O(n)`, `pop`/`top`/`empty` `O(1)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
class MyStack {
    queue<int> q;
public:
    void push(int x) {
        q.push(x);
        for (int i = 0; i < (int)q.size() - 1; i++) {   // rotate so x is at the front
            q.push(q.front());
            q.pop();
        }
    }
    int pop() { int v = q.front(); q.pop(); return v; }
    int top() { return q.front(); }
    bool empty() { return q.empty(); }
};
```

## 7. Dry Run

push 1 → `[1]`

push 2 → `[1,2]` → rotate once → `[2,1]`

push 3 → `[2,1,3]` → rotate twice → `[3,2,1]`

`top()` = `3`, `pop()` = `3`, queue `[2,1]`

## 8. Edge Cases

- `pop`/`top` on empty (assume valid calls; otherwise guard)
- Single element
- Alternating push/pop
- Cheap-push / costly-pop variant also exists

## Likely Follow-ups

- Make `pop` cheap instead of `push`? → Possible with the opposite trade-off.
- Two-queue version? → Same complexity, more memory.
