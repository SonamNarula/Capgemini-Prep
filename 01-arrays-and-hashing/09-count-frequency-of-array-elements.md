# Count Frequency of Array Elements

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Hash map / in-place counting |
| **Problem** | Count Frequency of Array Elements *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Count how many times each distinct element occurs and print `element → count`.

**Clarify:** Preserve first-appearance order or sorted order? Range of values? (If values are in `[1, N]`, an `O(1)`-space trick exists.)

## 2. Brute Force

For each element, scan the whole array to count it (skip already-counted values with a visited array).

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(n)`.

## 4. Bottleneck

Counting by repeated scanning; a lookup table makes each increment `O(1)`.

## 5. Optimal Approach (and why it works)

**General:** `unordered_map<int,int>`; increment per element. To keep first-appearance order, store order in a separate vector. **If values lie in `[1, N]` (N = size):** encode counts in place. Decrement values to `[0, N-1]`, then for each element `a[i] % N` add `N` to `a[a[i] % N]`. Afterwards `a[i] / N` is the count of value `i+1`.

**Complexity:** Map: Time `O(n)`, Space `O(d)`. In-place trick: Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
// General version: keeps first-appearance order
vector<pair<int,int>> countFrequency(const vector<int>& a) {
    unordered_map<int, int> freq;
    vector<int> order;
    for (int x : a) {
        if (!freq.count(x)) order.push_back(x);
        freq[x]++;
    }
    vector<pair<int,int>> res;
    for (int x : order) res.push_back({x, freq[x]});
    return res;
}

// In-place version: only if every value is in [1, N], N = a.size()
void freqInPlace(vector<int> a) {
    int n = a.size();
    for (int i = 0; i < n; i++) a[i]--;                // values now 0..n-1
    for (int i = 0; i < n; i++) a[a[i] % n] += n;      // a[i] % n recovers original value
    for (int i = 0; i < n; i++)
        if (a[i] / n > 0) cout << i + 1 << " -> " << a[i] / n << "\n";
}
```

## 7. Dry Run

`a = [2, 3, 2, 3, 5]` (N=5), in-place version

- After decrement: `[1,2,1,2,4]`
- `i=0`: `a[1]+=5` → `[1,7,1,2,4]`
- `i=1`: `7%5=2` → `a[2]+=5` → `[1,7,6,2,4]`
- `i=2`: `6%5=1` → `a[1]+=5` → `[1,12,6,2,4]`
- `i=3`: `2` → `a[2]+=5` → `[1,12,11,2,4]`
- `i=4`: `4` → `a[4]+=5` → `[1,12,11,2,9]`
- Counts: value 2 → `12/5=2`, value 3 → `11/5=2`, value 5 → `9/5=1`

## 8. Edge Cases

- Empty array → nothing to print
- All elements identical
- All distinct
- Negative / out-of-range values → in-place trick is invalid, use the map
- Large values: map handles any range

## Likely Follow-ups

- `map` vs `unordered_map` output order? → `map` gives sorted keys in `O(log n)`.
- Range is small (e.g. 0–100)? → A plain array counter is fastest.
