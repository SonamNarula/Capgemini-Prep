# Encode and Decode Strings

| | |
|---|---|
| **Section** | 3. Strings & Matrix |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Length-prefixed encoding |
| **Problem** | Encode and Decode Strings *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Design `encode(vector<string>) → string` and `decode(string) → vector<string>` so decode perfectly reconstructs the original list. Strings can contain any characters, including delimiters.

**Clarify:** Can strings be empty? Any restriction on characters?

## 2. Brute Force

Join with a delimiter such as `,` or `#`, split on decode.

## 3. Brute-Force Complexity

Time `O(total length)`, Space `O(total length)`. **Incorrect** when the delimiter appears inside a string.

## 4. Bottleneck

Delimiters can collide with content. We need an unambiguous framing that doesn't depend on characters inside the string.

## 5. Optimal Approach (and why it works)

Prefix each string with its **length** and a separator: `len#string`. When decoding, read digits up to `#` to get the length, then take exactly that many characters, no matter what they contain. Move on to the next chunk.

**Complexity:** Time `O(total length)`, Space `O(total length)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
string encode(const vector<string>& strs) {
    string out;
    for (const string& s : strs)
        out += to_string(s.size()) + "#" + s;
    return out;
}

vector<string> decode(const string& s) {
    vector<string> res;
    int i = 0;
    while (i < (int)s.size()) {
        int j = s.find('#', i);               // end of the length field
        int len = stoi(s.substr(i, j - i));
        res.push_back(s.substr(j + 1, len));
        i = j + 1 + len;
    }
    return res;
}
```

## 7. Dry Run

Input `["lint", "co#de"]`

- Encode → `4#lint5#co#de`
- Decode: `i=0`, first `#` at 1, len `4` → `lint`, `i=6`
- `i=6`: first `#` at 7, len `5` → `co#de` (the inner `#` is harmless because we read exactly 5 chars)

## 8. Edge Cases

- Empty list → `""` → decode gives `[]`
- Empty string in list → `0#`
- Strings containing `#` or digits
- Very long strings (length field has multiple digits)

## Likely Follow-ups

- Why is the length-prefix safe even with `#` inside strings? → We never *search* inside the payload, we skip by length.
- Alternative? → Escape characters, but length-prefix is simpler.
