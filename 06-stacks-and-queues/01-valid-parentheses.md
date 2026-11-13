# Valid Parentheses

| | |
|---|---|
| **Section** | 6. Stacks & Queues |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Stack |
| **Problem** | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given a string of `()[]{}` characters, check whether brackets are closed by the same type and in the correct order.

**Clarify:** Only bracket characters? Empty string valid?

## 2. Brute Force

Repeatedly delete adjacent matching pairs `()`, `[]`, `{}` until nothing changes; valid if the string becomes empty.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(n)`.

## 4. Bottleneck

Repeated rescanning. The most recent unmatched opener must match the next closer, which is exactly LIFO behaviour.

## 5. Optimal Approach (and why it works)

Push openers. On a closer, the stack must be non-empty and its top must be the matching opener; pop it. At the end the stack must be empty.

**Complexity:** Time `O(n)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') {
            st.push(c);
        } else {
            if (st.empty()) return false;
            char t = st.top(); st.pop();
            if ((c == ')' && t != '(') ||
                (c == '}' && t != '{') ||
                (c == ']' && t != '[')) return false;
        }
    }
    return st.empty();
}
```

## 7. Dry Run

`"{[]}"`

- `{` push, `[` push, `]` pops `[` ✓, `}` pops `{` ✓ → stack empty → `true`

`"(]"`: `(` push, `]` vs top `(` ✗ → `false`

## 8. Edge Cases

- Empty string → `true`
- Only openers `"((("` → stack non-empty → `false`
- Only closer `")"` → empty-stack check
- Odd length → always `false` (quick pre-check)
- Interleaved `"([)]"` → `false`

## Likely Follow-ups

- Minimum add/remove to make valid? → Counters or stack of indices.
- Include other characters? → Ignore non-bracket characters.
