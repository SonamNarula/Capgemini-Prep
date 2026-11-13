# Move All `#` Characters to Front

| | |
|---|---|
| **Section** | 3. Strings & Matrix |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Single pass / backward overwrite |
| **Problem** | Move All `#` Characters to Front *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Move every `#` in a string to the front while keeping the relative order of all other characters, in place.

**Clarify:** Must the other characters stay in the same order? (Assumed yes.)

## 2. Brute Force

Build a new string: first all `#`, then the other characters in order.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

Extra string; also forward overwrite would destroy unread characters.

## 5. Optimal Approach (and why it works)

Scan from the **right** with `read`, write non-`#` characters from the right with `write`. Because `write ≥ read` always, nothing unread is overwritten. When the scan finishes, fill the remaining positions `[0..write]` with `#`.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void moveHashToFront(string& s) {
    int write = (int)s.size() - 1;
    for (int read = (int)s.size() - 1; read >= 0; read--)
        if (s[read] != '#') s[write--] = s[read];
    while (write >= 0) s[write--] = '#';
}
```

## 7. Dry Run

`"ab#c#d"`

- `read=5 'd'` → `s[5]='d'`, `write=4`
- `read=4 '#'` skip
- `read=3 'c'` → `s[4]='c'`, `write=3`
- `read=2 '#'` skip
- `read=1 'b'` → `s[3]='b'`, `write=2`
- `read=0 'a'` → `s[2]='a'`, `write=1`
- Fill `s[1]`,`s[0]` with `#` → `"##abcd"`

## 8. Edge Cases

- No `#` → unchanged
- All `#` → unchanged
- Empty string
- `#` already at the front
- Single character

## Likely Follow-ups

- Move `#` to the *end*? → Mirror it: scan from the left.
- Same as 'move zeros' in disguise: identify that pattern in the interview.
