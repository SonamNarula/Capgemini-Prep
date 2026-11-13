# Valid Palindrome

| | |
|---|---|
| **Section** | 2. Two Pointers & Sliding Window |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Left/right pointers |
| **Problem** | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

After lowercasing and ignoring non-alphanumeric characters, check whether the string reads the same forwards and backwards.

**Clarify:** Is an empty string a palindrome? (Yes.) Only ASCII?

## 2. Brute Force

Build a cleaned, lowercased copy, reverse it, compare.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

Not a time problem; the extra copy is the only waste.

## 5. Optimal Approach (and why it works)

Two pointers `l` and `r` from both ends. Skip non-alphanumeric characters on each side, compare lowercased characters, move inward. Stop when pointers cross.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
bool isPalindrome(string s) {
    int l = 0, r = (int)s.size() - 1;
    while (l < r) {
        while (l < r && !isalnum((unsigned char)s[l])) l++;
        while (l < r && !isalnum((unsigned char)s[r])) r--;
        if (tolower((unsigned char)s[l]) != tolower((unsigned char)s[r])) return false;
        l++; r--;
    }
    return true;
}
```

## 7. Dry Run

`"race a car"` → cleaned `raceacar`

- `r`/`r` ✓, `a`/`a` ✓, `c`/`c` ✓, `e` vs `a` ✗ → `false`

`"A man, a plan"`-style input with punctuation: pointers skip spaces and commas before comparing.

## 8. Edge Cases

- Empty string or only symbols `".,"` → `true`
- Single character
- Mixed case `"Aa"` → `true`
- Digits count as alphanumeric: `"0P"` → `false`

## Likely Follow-ups

- Palindrome after deleting at most one character (LC 680)? → On mismatch try skipping `l` or `r`.
- Why cast to `unsigned char`? → `isalnum` is undefined for negative `char` values.
