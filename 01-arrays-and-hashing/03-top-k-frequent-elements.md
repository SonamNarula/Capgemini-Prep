# Top K Frequent Elements

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Bucket sort / min-heap |
| **Problem** | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the `k` most frequent elements of `nums`. Answer in any order; the answer is guaranteed unique.

**Clarify:** Can `k` exceed the number of distinct values? Are ties possible at the boundary? (Guaranteed not here.) Target complexity better than `O(n log n)`?

*This problem appears in both Section 1 and Section 7 of the sheet; both solutions are here.*

## 2. Brute Force

Count frequencies with a map, copy `(value, freq)` pairs into a vector, sort by frequency descending, take the first `k`.

## 3. Brute-Force Complexity

Time `O(n + d log d)` where `d` = distinct values (≤ `O(n log n)`), Space `O(d)`.

## 4. Bottleneck

Fully sorting all `d` pairs when we only need the top `k` is wasted work.

## 5. Optimal Approach (and why it works)

**Bucket sort (best):** a frequency can be at most `n`, so create `n+1` buckets where `bucket[f]` holds values that appear `f` times. Scan buckets from high to low until `k` values are collected. Everything is linear.

**Min-heap (also expected):** keep a min-heap of size `k` on `(freq, value)`; push each pair, pop when size exceeds `k`. Whatever remains is the top `k`.

**Complexity:** Bucket: Time `O(n)`, Space `O(n)`. Heap: Time `O(n log k)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
// Approach 1: bucket sort, O(n)
vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int, int> freq;
    for (int x : nums) freq[x]++;

    int n = nums.size();
    vector<vector<int>> bucket(n + 1);       // bucket[f] = values with frequency f
    for (auto& kv : freq) bucket[kv.second].push_back(kv.first);

    vector<int> res;
    for (int f = n; f >= 1 && (int)res.size() < k; f--)
        for (int v : bucket[f]) {
            res.push_back(v);
            if ((int)res.size() == k) break;
        }
    return res;
}

// Approach 2: min-heap of size k, O(n log k)
vector<int> topKFrequentHeap(vector<int>& nums, int k) {
    unordered_map<int, int> freq;
    for (int x : nums) freq[x]++;

    using P = pair<int, int>;                // (freq, value)
    priority_queue<P, vector<P>, greater<P>> pq;
    for (auto& kv : freq) {
        pq.push({kv.second, kv.first});
        if ((int)pq.size() > k) pq.pop();    // drop least frequent
    }
    vector<int> res;
    while (!pq.empty()) { res.push_back(pq.top().second); pq.pop(); }
    return res;
}
```

## 7. Dry Run

`nums = [1,1,1,2,2,3]`, `k = 2`

- freq: `{1:3, 2:2, 3:1}`
- buckets: `[1]→{3}`, `[2]→{2}`, `[3]→{1}`
- scan from `f=6` down: `f=3` → push `1`; `f=2` → push `2`; size = `k` → result `[1, 2]`

## 8. Edge Cases

- `k` equals the number of distinct values → return all of them
- Single element `[1]`, `k=1`
- All elements the same
- All elements unique (every frequency is 1)
- Negative numbers (fine, they're only map keys)

## Likely Follow-ups

- Why is a *min*-heap used for *top* K? → Smallest of the kept K sits at the top, so eviction is `O(log k)`.
- What if data is a stream? → Maintain the map plus heap incrementally.
- Quickselect alternative? → Average `O(n)` on frequencies.
