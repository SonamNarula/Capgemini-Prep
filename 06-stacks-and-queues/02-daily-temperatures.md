# Daily Temperatures

| | |
|---|---|
| **Section** | 6. Stacks & Queues |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Monotonic stack |
| **Problem** | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

For each day, return how many days you must wait for a warmer temperature; `0` if never.

**Clarify:** Strictly warmer? Array size up to `1e5`?

## 2. Brute Force

For each day scan forward until a larger value appears.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

Each day re-scans days that earlier days already looked at. A stack of unresolved days answers all in one pass.

## 5. Optimal Approach (and why it works)

Keep a stack of **indices** with decreasing temperatures. For day `i`, while the stack top is colder than `t[i]`, that day has found its warmer day: `res[top] = i - top`, pop. Then push `i`. Days left on the stack get `0`.

**Complexity:** Time `O(n)` (each index pushed/popped once), Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> dailyTemperatures(vector<int>& t) {
    int n = t.size();
    vector<int> res(n, 0);
    stack<int> st;                            // indices, temperatures decreasing
    for (int i = 0; i < n; i++) {
        while (!st.empty() && t[i] > t[st.top()]) {
            res[st.top()] = i - st.top();
            st.pop();
        }
        st.push(i);
    }
    return res;
}
```

## 7. Dry Run

`[73,74,75,71,69,72,76,73]`

- `i=1 (74)`: pops `0` → `res[0]=1`
- `i=2 (75)`: pops `1` → `res[1]=1`
- `i=3,4`: pushed (71, 69)
- `i=5 (72)`: pops `4` (`res=1`), pops `3` (`res=2`), push `5`
- `i=6 (76)`: pops `5` (`1`), pops `2` (`4`), push `6`
- `i=7 (73)`: push
- Result `[1,1,4,2,1,1,0,0]`

## 8. Edge Cases

- Strictly decreasing → all `0`
- Strictly increasing → all `1` except last `0`
- All equal → all `0` (strict `>`)
- Single element → `[0]`

## Likely Follow-ups

- Why indices, not values? → We need the distance.
- Next Smaller Element? → Flip the comparison.
