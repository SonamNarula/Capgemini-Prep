# Letter Combinations of a Phone Number

| | |
|---|---|
| **Section** | 9. Recursion & Backtracking |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Recursive choices |
| **Problem** | [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given digits `2-9`, return all letter combinations they could represent on a phone keypad.

**Clarify:** Empty input → `[]`? Digits `0`/`1` excluded?

## 2. Brute Force

Iteratively build combinations using nested loops, which only works when the digit count is fixed.

## 3. Brute-Force Complexity

Not generalisable.

## 4. Bottleneck

Number of nested loops depends on input length, so recursion (or an iterative queue) is the right tool.

## 5. Optimal Approach (and why it works)

Map each digit to its letters. Recurse over digit positions: for each letter of the current digit, append, recurse to the next digit, then remove (backtrack). When all digits are used, record the string.

**Complexity:** Time `O(4ⁿ · n)`, Space `O(n)` recursion + output.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
void dfs(int i, const string& digits, string& cur, vector<string>& res) {
    static const vector<string> mp =
        {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    if (i == (int)digits.size()) { res.push_back(cur); return; }
    for (char c : mp[digits[i] - '0']) {
        cur.push_back(c);
        dfs(i + 1, digits, cur, res);
        cur.pop_back();
    }
}

vector<string> letterCombinations(string digits) {
    if (digits.empty()) return {};
    vector<string> res;
    string cur;
    dfs(0, digits, cur, res);
    return res;
}
```

## 7. Dry Run

`digits = "23"`

- `2 → a,b,c`; `3 → d,e,f`
- `a` + `d,e,f` → `ad ae af`
- `b` + `d,e,f` → `bd be bf`
- `c` + `d,e,f` → `cd ce cf`
- 9 combinations

## 8. Edge Cases

- Empty string → `[]` (not `[""]`)
- Single digit `"2"` → `["a","b","c"]`
- Digits `7` and `9` have 4 letters
- Maximum length is small (≤ 4 in LeetCode)

## Likely Follow-ups

- Iterative BFS-style solution with a queue?
- What if `0`/`1` can appear?
