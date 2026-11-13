# Check If One String Is Rotation of Another

| | |
|---|---|
| **Section** | 3. Strings & Matrix |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | `s1 + s1`, substring search |
| **Problem** | Check If One String Is Rotation of Another *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Check whether `s2` can be obtained by rotating `s1` (e.g. `abcd` → `cdab`).

**Clarify:** Case-sensitive? Are different lengths automatically false? (Yes.)

## 2. Brute Force

Try every rotation offset of `s1` (`n` rotations, each `O(n)` to build and compare).

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(n)`.

## 4. Bottleneck

Generating each rotation explicitly. All rotations of `s1` are already substrings of `s1 + s1`.

## 5. Optimal Approach (and why it works)

If lengths differ → `false`. Otherwise `s2` is a rotation of `s1` iff `s2` appears as a substring of `s1 + s1`. Use `find`; use KMP if a guaranteed `O(n)` bound is needed.

**Complexity:** Time `O(n)` with KMP (`std::string::find` is typically fast but not guaranteed linear), Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
bool isRotation(const string& a, const string& b) {
    if (a.size() != b.size()) return false;
    return (a + a).find(b) != string::npos;
}
```

## 7. Dry Run

`a = "abcd"`, `b = "cdab"`

- `a + a = "abcdabcd"`
- `"cdab"` found at index 2 → `true`

`a = "abcd"`, `b = "acbd"` → not found → `false`

## 8. Edge Cases

- Different lengths → `false`
- Both empty → `true`
- Identical strings → `true` (rotation by 0)
- Repeated patterns `"aaaa"`
- Single character

## Likely Follow-ups

- Why does doubling work? → Any rotation is a window of length `n` inside `a+a`.
- Guarantee linear time? → KMP / Z-function on `b + '#' + a + a`.
