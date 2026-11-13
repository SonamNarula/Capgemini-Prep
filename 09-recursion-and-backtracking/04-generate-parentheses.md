# Generate Parentheses

| | |
|---|---|
| **Section** | 9. Recursion & Backtracking |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Constraint-based recursion |
| **Problem** | [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Generate all well-formed strings with `n` pairs of parentheses.

**Clarify:** Only `()`? Output size is the Catalan number `Cₙ`.

## 2. Brute Force

Generate all `2^(2n)` strings of `(` and `)`, then validate each.

## 3. Brute-Force Complexity

Time `O(2^(2n) · n)`.

## 4. Bottleneck

Most generated strings are invalid. We can prune invalid prefixes immediately.

## 5. Optimal Approach (and why it works)

Build the string character by character with counters `open` and `close`. Rules: add `(` if `open < n`; add `)` if `close < open`. These two rules guarantee validity. Record when length reaches `2n`.

**Complexity:** Time `O(4ⁿ / √n)` (Catalan), Space `O(n)` recursion + output.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(int open, int close, int n, string& cur, vector<string>& res) {
    if ((int)cur.size() == 2 * n) { res.push_back(cur); return; }
    if (open < n) {
        cur.push_back('(');
        dfs(open + 1, close, n, cur, res);
        cur.pop_back();
    }
    if (close < open) {
        cur.push_back(')');
        dfs(open, close + 1, n, cur, res);
        cur.pop_back();
    }
}

vector<string> generateParenthesis(int n) {
    vector<string> res;
    string cur;
    dfs(0, 0, n, cur, res);
    return res;
}
```

## 7. Dry Run

`n = 2`

- `(` → `((` → `(()` → `(())` ✓
- `(` → `()` → `()(` → `()()` ✓
- `((` cannot add a third `(` (`open == n`)
- Result `["(())","()()"]`

## 8. Edge Cases

- `n = 1` → `["()"]`
- `n = 0` → `[""]` (if allowed)
- Never allow `close > open` at any prefix
- Count equals Catalan number: n=3 → 5

## Likely Follow-ups

- Valid Parentheses check vs generation?
- Why does the `close < open` rule ensure validity?
