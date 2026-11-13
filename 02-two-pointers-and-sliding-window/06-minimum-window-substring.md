# Minimum Window Substring

| | |
|---|---|
| **Section** | 2. Two Pointers & Sliding Window |
| **Priority** | P0 |
| **Difficulty** | Hard |
| **Key pattern** | Dynamic window + frequency |
| **Problem** | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given `s` and `t`, find the smallest substring of `s` that contains every character of `t` (with multiplicity). Return `""` if none exists.

**Clarify:** Case-sensitive? Duplicates in `t` need matching counts? Is the answer unique? (Guaranteed in LC.)

## 2. Brute Force

Check every substring of `s` against the frequency of `t`.

## 3. Brute-Force Complexity

Time `O(n² · m)` or so, Space `O(1)`/`O(m)`.

## 4. Bottleneck

Substrings overlap heavily. A valid window can be shrunk from the left while it stays valid; an invalid one only needs expanding on the right.

## 5. Optimal Approach (and why it works)

Count required characters in `need[]`. Expand `r`: decrement `need[s[r]]`; if it was positive the window gained a needed char → `missing--`. While `missing == 0` the window is valid: record it, then shrink from the left (increment `need[s[l]]`; if it becomes positive we lost a required char → `missing++`).

**Complexity:** Time `O(n + m)`, Space `O(1)` (fixed charset).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
string minWindow(string s, string t) {
    if (t.size() > s.size()) return "";
    vector<int> need(256, 0);
    for (unsigned char c : t) need[c]++;

    int missing = t.size();                   // chars still needed
    int l = 0, bestStart = 0, bestLen = INT_MAX;
    for (int r = 0; r < (int)s.size(); r++) {
        if (need[(unsigned char)s[r]]-- > 0) missing--;
        while (missing == 0) {                // valid window: try to shrink
            if (r - l + 1 < bestLen) { bestLen = r - l + 1; bestStart = l; }
            if (++need[(unsigned char)s[l]] > 0) missing++;
            l++;
        }
    }
    return bestLen == INT_MAX ? "" : s.substr(bestStart, bestLen);
}
```

## 7. Dry Run

`s = "ADOBECODEBANC"`, `t = "ABC"`

- Expand until `ADOBEC` (r=5): `missing=0`, record length 6, shrink: drop `A` → `missing=1`
- Continue expanding; later window `CODEBA` (len 6) then shrinking gives `ODEBANC`→`DEBANC`→`EBANC`→`BANC`
- Final `BANC` (length 4) is the minimum

## 8. Edge Cases

- `t` longer than `s` → `""`
- `t` has duplicates `"AA"` — needs two `A`s
- No valid window → `""`
- `s == t`
- Single-character strings

## Likely Follow-ups

- Why is `missing` an int instead of checking all 256 counts each time? → `O(1)` validity check.
- Shortest *subsequence* version? → Different problem, DP or two-pointer expansion.
