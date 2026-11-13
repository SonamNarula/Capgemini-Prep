# Two Sum

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | `unordered_map`, single-pass lookup |
| **Problem** | [Two Sum](https://leetcode.com/problems/two-sum/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Given an array `nums` and an integer `target`, return the indices of the two numbers that add up to `target`. Exactly one solution exists; the same element can't be used twice.

**Clarify:** Can there be negatives or duplicates? Is the answer's order important? What are the size limits?

## 2. Brute Force

Try every pair `(i, j)` with `i < j` and check `nums[i] + nums[j] == target`.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

For each element we *search* for its complement with a linear scan. That inner search is the cost; a hash map makes it `O(1)`.

## 5. Optimal Approach (and why it works)

Walk the array once. For `nums[i]`, the number we need is `target - nums[i]`. Look it up in a map of *already seen* values (`value → index`). If present, we're done; otherwise store `nums[i]`. Because we store *after* checking, an element never pairs with itself.

**Complexity:** Time `O(n)` average, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> seen;            // value -> index
    for (int i = 0; i < (int)nums.size(); i++) {
        int need = target - nums[i];
        auto it = seen.find(need);
        if (it != seen.end()) return {it->second, i};
        seen[nums[i]] = i;
    }
    return {};                               // problem guarantees a solution
}
```

## 7. Dry Run

`nums = [2, 7, 11, 15]`, `target = 9`

- `i=0`: need `7`, not in map → store `{2:0}`
- `i=1`: need `2`, found at index `0` → return `[0, 1]`

## 8. Edge Cases

- Duplicates: `[3, 3]`, target `6` → `[0, 1]` (works because we check before inserting)
- Negative numbers and zero: `[-3, 4, 3, 90]`, target `0`
- Two elements only
- If `nums[i]` can be near `INT_MAX/INT_MIN`, compute `target - nums[i]` in `long long`

## Likely Follow-ups

- What if the array is sorted? → Two pointers, `O(1)` space.
- What if there are many valid pairs and you need all? → Track counts, handle duplicates carefully.
- Why `unordered_map` over `map`? → Average `O(1)` vs `O(log n)`; mention worst-case `O(n)` on collisions.
