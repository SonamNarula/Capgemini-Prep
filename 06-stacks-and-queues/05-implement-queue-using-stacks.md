# Implement Queue Using Stacks

| | |
|---|---|
| **Section** | 6. Stacks & Queues |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Stack |
| **Problem** | [Implement Queue Using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Implement a FIFO queue (`push`, `pop`, `peek`, `empty`) using only stack operations.

**Clarify:** Is amortised `O(1)` acceptable?

## 2. Brute Force

On every `push`, move everything to a second stack, push, and move back: `O(n)` per push.

## 3. Brute-Force Complexity

`push` `O(n)`, `pop` `O(1)`.

## 4. Bottleneck

Moving all elements back and forth on each push is wasteful. Do it lazily and only when needed.

## 5. Optimal Approach (and why it works)

Two stacks: `in` (receives pushes) and `out` (serves pops/peeks). When `out` is empty, pour **all** of `in` into `out`, which reverses order so the oldest element is on top. Each element moves at most twice, so operations are amortised `O(1)`.

**Complexity:** Amortised `O(1)` per operation, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
class MyQueue {
    stack<int> in, out;
    void shift() {
        if (out.empty())
            while (!in.empty()) { out.push(in.top()); in.pop(); }
    }
public:
    void push(int x) { in.push(x); }
    int pop() { shift(); int v = out.top(); out.pop(); return v; }
    int peek() { shift(); return out.top(); }
    bool empty() { return in.empty() && out.empty(); }
};
```

## 7. Dry Run

push 1, push 2, push 3 → `in = [1,2,3]`

`pop()`: `out` empty → pour → `out` top-to-bottom `1,2,3` → pop `1`

push 4 → `in = [4]`; `pop()` → `out` not empty → `2` (FIFO preserved)

## 8. Edge Cases

- Pop right after push (single element)
- Interleaved push/pop (don't pour while `out` is non-empty)
- `empty()` must check both stacks
- Calls on empty (assume valid per problem)

## Likely Follow-ups

- Why amortised `O(1)`? → Each element is pushed and popped at most twice in total.
- Worst single pop is `O(n)`; explain the difference vs amortised.
