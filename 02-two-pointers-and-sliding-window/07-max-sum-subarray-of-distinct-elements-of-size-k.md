# Max Sum Subarray of Distinct Elements of Size K

| | |
|---|---|
| **Section** | 2. Two Pointers & Sliding Window |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Fixed window + frequency map |
| **Problem** | Max Sum Subarray of Distinct Elements of Size K *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Among all subarrays of length exactly `k` whose elements are **all distinct**, return the maximum sum. Return `-1` (or `0`, as the statement says) if no such window exists.

**Clarify:** What to return when none exists? Negatives allowed? `k > n`?

## 2. Brute Force

For every window of size `k`, build a set to check distinctness and sum the elements.

## 3. Brute-Force Complexity

Time `O(n·k)`, Space `O(k)`.

## 4. Bottleneck

Each new window recomputes the sum and set from scratch though it differs by just two elements.

## 5. Optimal Approach (and why it works)

Fixed-size sliding window with a frequency map. Add `a[i]`; when the window exceeds `k`, remove `a[i-k]` (erase the key when its count hits 0). A window is distinct exactly when `freq.size() == k`. Maintain a running sum and update the best for distinct windows.

**Complexity:** Time `O(n)`, Space `O(k)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
long long maxSumDistinct(const vector<int>& a, int k) {
    int n = a.size();
    if (k > n) return -1;
    unordered_map<int, int> freq;
    long long sum = 0, best = LLONG_MIN;
    for (int i = 0; i < n; i++) {
        freq[a[i]]++;
        sum += a[i];
        if (i >= k) {                                     // slide: remove a[i-k]
            int out = a[i - k];
            sum -= out;
            if (--freq[out] == 0) freq.erase(out);
        }
        if (i >= k - 1 && (int)freq.size() == k)          // k distinct values
            best = max(best, sum);
    }
    return best == LLONG_MIN ? -1 : best;
}
```

## 7. Dry Run

`a = [1,5,4,2,9,9,9]`, `k = 3`

- `[1,5,4]` sum 10, 3 distinct ✓
- `[5,4,2]` sum 11 ✓
- `[4,2,9]` sum 15 ✓ (best)
- `[2,9,9]` only 2 distinct ✗
- `[9,9,9]` 1 distinct ✗
- Answer `15`

## 8. Edge Cases

- `k > n` → no window
- No window with distinct elements → `-1`
- `k = 1` → max element
- All elements equal
- Negative values (hence `LLONG_MIN` sentinel instead of `0`)

## Likely Follow-ups

- Why is `freq.size() == k` a correct distinct test? → Window holds exactly `k` elements; `k` distinct keys means no repeats.
- Variable window version? → Longest subarray with all distinct elements.
