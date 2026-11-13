# Next Greater Element to Right

| | |
|---|---|
| **Section** | 6. Stacks & Queues |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Monotonic stack |
| **Problem** | Next Greater Element to Right *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

For every element, find the first element to its **right** that is strictly greater; output `-1` if none.

**Clarify:** Strictly greater? Circular array? (Assumed no.)

## 2. Brute Force

For each index scan right until a greater element appears.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

Repeated scanning again. Smaller elements already passed can never be an answer for anything to their left.

## 5. Optimal Approach (and why it works)

Traverse **right to left** with a stack of candidate values. For `a[i]`, pop all values `≤ a[i]` (they're useless now). The top (if any) is the answer. Push `a[i]`.

**Complexity:** Time `O(n)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> nextGreater(const vector<int>& a) {
    int n = a.size();
    vector<int> res(n, -1);
    stack<int> st;                            // candidate values, top is nearest
    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && st.top() <= a[i]) st.pop();
        if (!st.empty()) res[i] = st.top();
        st.push(a[i]);
    }
    return res;
}
```

## 7. Dry Run

`[4, 5, 2, 25]`

- `i=3 (25)`: stack empty → `-1`, push 25
- `i=2 (2)`: top 25 > 2 → `25`, push 2
- `i=1 (5)`: pop 2, top 25 → `25`, push 5
- `i=0 (4)`: top 5 > 4 → `5`
- Result `[5, 25, 25, -1]`

## 8. Edge Cases

- Strictly decreasing array → all `-1`
- Strictly increasing → each is the next element, last `-1`
- Duplicates `[2,2,2]` → all `-1` (we pop `≤`)
- Single element
- Circular variant (LC 503): iterate `2n` times with `i % n`

## Likely Follow-ups

- Next greater element to the *left*? → Traverse left to right.
- Stock span / histogram problems use the same idea.
