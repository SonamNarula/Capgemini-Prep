# Run-Length String Compression

| | |
|---|---|
| **Section** | 3. Strings & Matrix |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Two pointers |
| **Problem** | Run-Length String Compression *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Compress consecutive repeated characters: `"aaabbc"` → `"a3b2c1"`.

**Clarify:** Should single characters get a `1` (`c1`) or stay as `c`? Return the longer original if the compressed form isn't shorter? *(Here: always write the count.)*

## 2. Brute Force

Use a counter and a result string while scanning; compare each character with the previous one.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)` for the output.

## 4. Bottleneck

Nothing to improve; the output size itself needs `O(n)`. The aim is a clean single pass.

## 5. Optimal Approach (and why it works)

Pointer `i` marks a run's start. Advance `j` while `s[j] == s[i]`. Append `s[i]` and `j - i`. Set `i = j`.

**Complexity:** Time `O(n)`, Space `O(n)` (output).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
string compress(const string& s) {
    string res;
    int n = s.size(), i = 0;
    while (i < n) {
        int j = i;
        while (j < n && s[j] == s[i]) j++;    // run is s[i..j-1]
        res += s[i];
        res += to_string(j - i);
        i = j;
    }
    return res;
}
// Variant: skip the count when run length is 1 -> "aaabbc" gives "a3b2c"
```

## 7. Dry Run

`"aaabbc"`

- `i=0`: run of `a` up to `j=3` → `a3`
- `i=3`: run of `b` up to `j=5` → `b2`
- `i=5`: run of `c`, `j=6` → `c1`
- Result `"a3b2c1"`

## 8. Edge Cases

- Empty string → `""`
- Single character → `"a1"`
- All same `"aaaa"` → `"a4"`
- No repeats `"abc"` → `"a1b1c1"` (longer than input!)
- Run length ≥ 10 needs multi-digit count (`to_string` handles it)

## Likely Follow-ups

- In-place version (LC 443)? → Write pointer on the same array, skip `1`, write digits one by one.
- Decode the compressed string? → Parse char then number.
