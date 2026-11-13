# Min Stack

| | |
|---|---|
| **Section** | 6. Stacks & Queues |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Auxiliary stack |
| **Problem** | [Min Stack](https://leetcode.com/problems/min-stack/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Design a stack supporting `push`, `pop`, `top` and `getMin` all in `O(1)`.

**Clarify:** Duplicates? Calls on an empty stack (assume valid)?

## 2. Brute Force

Scan the stack for the minimum on every `getMin`.

## 3. Brute-Force Complexity

`getMin` `O(n)`.

## 4. Bottleneck

The minimum changes only on push/pop at the top, so it can be remembered alongside.

## 5. Optimal Approach (and why it works)

Use a second stack of minima. On `push(x)`, also push `x` to `mn` if `mn` is empty or `x <= mn.top()` (use `<=` so duplicates of the minimum are tracked). On `pop`, if the popped value equals `mn.top()`, pop `mn` too. `getMin` is `mn.top()`.

*Alternative:* store pairs `(value, minSoFar)` in one stack.

**Complexity:** All operations `O(1)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
class MinStack {
    stack<int> st, mn;
public:
    void push(int x) {
        st.push(x);
        if (mn.empty() || x <= mn.top()) mn.push(x);
    }
    void pop() {
        if (st.top() == mn.top()) mn.pop();
        st.pop();
    }
    int top() { return st.top(); }
    int getMin() { return mn.top(); }
};
```

## 7. Dry Run

push `-2`, `0`, `-3`

- `st = [-2,0,-3]`, `mn = [-2,-3]`
- `getMin()` → `-3`
- `pop()` removes `-3` (equals `mn.top()` → pop `mn` too)
- `top()` → `0`, `getMin()` → `-2`

## 8. Edge Cases

- Duplicate minimums `push 1, push 1, pop` → min must still be `1` (the reason for `<=`)
- Single element
- Negative numbers
- Strictly increasing pushes (min never changes)
- `INT_MIN` values (no arithmetic tricks used here, so safe)

## Likely Follow-ups

- Space-optimised version with a single stack? → Store `2*x - min` encoded values (mention only, risky).
- Also `getMax`? → Mirror with a max stack.
