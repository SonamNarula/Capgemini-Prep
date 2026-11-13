# Climbing Stairs

| | |
|---|---|
| **Section** | 10. Dynamic Programming |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | 1D DP |
| **Problem** | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

You can climb 1 or 2 steps at a time. In how many distinct ways can you reach step `n`?

**Clarify:** `n ≥ 1`? Does the answer fit in `int`? (Yes for `n ≤ 45`.)

## 2. Brute Force

Recursion `f(n) = f(n-1) + f(n-2)` with no memo.

## 3. Brute-Force Complexity

Time `O(2ⁿ)`, Space `O(n)` stack.

## 4. Bottleneck

Overlapping subproblems: `f(k)` is recomputed exponentially many times.

## 5. Optimal Approach (and why it works)

State: `f(i)` = ways to reach step `i`. Transition: `f(i) = f(i-1) + f(i-2)`. Base: `f(1)=1`, `f(2)=2`. Only the last two values are needed → two variables.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int climbStairs(int n) {
    if (n <= 2) return n;
    int a = 1, b = 2;                         // f(1), f(2)
    for (int i = 3; i <= n; i++) {
        int c = a + b;
        a = b;
        b = c;
    }
    return b;
}
```

## 7. Dry Run

`n = 5`

- `f(1)=1`, `f(2)=2`
- `f(3)=3`, `f(4)=5`, `f(5)=8`
- Answer `8`

## 8. Edge Cases

- `n = 1` → `1`
- `n = 2` → `2`
- `n = 45` is the max for `int`
- `n = 0` (if allowed) → `1`

## Likely Follow-ups

- Steps of 1, 2 or 3? → Add `f(i-3)`.
- Min cost climbing stairs?
- Fibonacci connection and matrix exponentiation `O(log n)`.
