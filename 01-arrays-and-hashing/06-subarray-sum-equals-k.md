# Subarray Sum Equals K

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Prefix sum + frequency map |
| **Problem** | [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Count the contiguous subarrays whose sum equals `k`.

**Clarify:** Negatives allowed? (Yes: this kills sliding window.) Can `k` be `0` or negative?

## 2. Brute Force

For every start `i`, extend `j` and keep a running sum; count whenever it equals `k`.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)`.

## 4. Bottleneck

We re-add overlapping ranges. Sliding window can't help because negative numbers break monotonicity.

## 5. Optimal Approach (and why it works)

Let `prefix[j]` = sum of `nums[0..j]`. Subarray `(i+1..j)` has sum `k` iff `prefix[j] - prefix[i] = k`, i.e. `prefix[i] = prefix[j] - k`. Keep a map `prefixSum → count of occurrences so far`. At each `j` add `map[prefix[j] - k]` to the answer, then record `prefix[j]`. Seed `map[0] = 1` so subarrays starting at index 0 are counted.

**Complexity:** Time `O(n)` average, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> cnt;
    cnt[0] = 1;                               // empty prefix
    int sum = 0, res = 0;
    for (int x : nums) {
        sum += x;
        auto it = cnt.find(sum - k);
        if (it != cnt.end()) res += it->second;
        cnt[sum]++;
    }
    return res;
}
```

## 7. Dry Run

`nums = [1, 1, 1]`, `k = 2`

- start `cnt={0:1}`
- `x=1`: sum=1, need `-1` → none; `cnt={0:1,1:1}`
- `x=1`: sum=2, need `0` → +1 (res=1); `cnt[2]=1`
- `x=1`: sum=3, need `1` → +1 (res=2)
- Answer `2`

## 8. Edge Cases

- Negative numbers `[1,-1,0]`, `k=0` → `3`
- `k = 0` with many zeros (count grows quadratically)
- Single element equal to `k`
- No valid subarray → `0`
- Large sums: use `long long` if constraints demand

## Likely Follow-ups

- Why seed `cnt[0]=1`? → Handles subarrays beginning at index 0.
- When does sliding window work instead? → Only if all numbers are non-negative.
