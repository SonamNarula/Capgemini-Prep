# Group Anagrams

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Frequency array / sorted key |
| **Problem** | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Group strings that are anagrams of each other. Output groups can be in any order.

**Clarify:** Only lowercase letters? Can strings be empty? Size of `n` and max string length `K`?

## 2. Brute Force

For each string, compare against every existing group's representative by sorting both (or counting letters). Linear scan of groups per string.

## 3. Brute-Force Complexity

Time about `O(n² · K log K)`, Space `O(n·K)`.

## 4. Bottleneck

Finding the right group by comparison is slow. We need a *canonical key* so anagrams land in the same bucket in `O(1)` lookup.

## 5. Optimal Approach (and why it works)

Anagrams share an identical sorted string, so **sorted string = key** in an `unordered_map<string, vector<string>>`. Alternative key: a 26-count signature (e.g. `"#1#0#0..."`), which makes key-building `O(K)` instead of `O(K log K)`.

**Complexity:** Sorted key: Time `O(n · K log K)`, Space `O(n·K)`. Count key: Time `O(n · K)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for (auto& s : strs) {
        string key = s;
        sort(key.begin(), key.end());        // canonical form
        mp[key].push_back(s);
    }
    vector<vector<string>> res;
    res.reserve(mp.size());
    for (auto& kv : mp) res.push_back(move(kv.second));
    return res;
}
```

## 7. Dry Run

`["eat","tea","tan","ate","nat","bat"]`

- `eat → aet`, `tea → aet`, `ate → aet` → group `[eat, tea, ate]`
- `tan → ant`, `nat → ant` → group `[tan, nat]`
- `bat → abt` → group `[bat]`

## 8. Edge Cases

- Empty string `""` → its own group `[""]`
- Single string
- All strings identical
- All strings distinct (n groups)
- Strings of different lengths can never be anagrams (keys differ automatically)

## Likely Follow-ups

- Faster key? → 26-length frequency signature, `O(K)`.
- Unicode input? → Use `map<char,int>` / frequency over the full character set.
- Why `move()` when collecting? → Avoids copying the vectors.
