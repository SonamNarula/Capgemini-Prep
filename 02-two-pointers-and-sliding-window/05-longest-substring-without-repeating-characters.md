# Longest Substring Without Repeating Characters

| | |
|---|---|
| **Section** | 2. Two Pointers & Sliding Window |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Variable sliding window |
| **Problem** | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Length of the longest substring with all distinct characters.

**Clarify:** Character set (ASCII/Unicode)? Case-sensitive? Empty string → `0`?

## 2. Brute Force

Enumerate every substring and check for duplicates with a set.

## 3. Brute-Force Complexity

Time `O(n³)` (or `O(n²)` with early break), Space `O(min(n, charset))`.

## 4. Bottleneck

Re-scanning overlapping substrings. If the window `[l..r]` is valid, only the new character `s[r+1]` can break it.

## 5. Optimal Approach (and why it works)

Sliding window `[l..r]` plus `last[c]` = last seen index of each character. When `s[r]` was last seen inside the window (`last[c] >= l`), jump `l` to `last[c] + 1`. Update `last[c] = r` and the best length.

**Complexity:** Time `O(n)`, Space `O(1)` (fixed charset of 256).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int lengthOfLongestSubstring(string s) {
    vector<int> last(256, -1);                // last index of each char
    int l = 0, best = 0;
    for (int r = 0; r < (int)s.size(); r++) {
        unsigned char c = s[r];
        if (last[c] >= l) l = last[c] + 1;    // duplicate inside window -> shrink
        last[c] = r;
        best = max(best, r - l + 1);
    }
    return best;
}
```

## 7. Dry Run

`"abcabcbb"`

- `r=0..2` (`abc`): window length 3, `l=0`
- `r=3` (`a`): last `a`=0 ≥ `l` → `l=1`, len 3
- `r=4` (`b`): last `b`=1 → `l=2`
- `r=5` (`c`): last `c`=2 → `l=3`
- `r=6` (`b`): last `b`=4 ≥ 3 → `l=5`, len 2
- `r=7` (`b`): last `b`=6 → `l=7`, len 1
- Best `3`

## 8. Edge Cases

- Empty string → `0`
- All same `"bbbb"` → `1`
- All unique → `n`
- `"abba"` → `2` (why `last[c] >= l` check matters; without it `l` would move backwards)
- Spaces and symbols

## Likely Follow-ups

- Return the substring itself? → Track `bestStart`.
- At most K distinct characters? → Window with a frequency map and a distinct counter.
