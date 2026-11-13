# Kth Largest Element in an Array

| | |
|---|---|
| **Section** | 7. Heaps & Priority Queues |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Min-heap of size K |
| **Problem** | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the `k`-th largest element in sorted order (not the k-th distinct).

**Clarify:** Duplicates count separately? May I modify the array? Is `O(n)` expected?

## 2. Brute Force

Sort descending and return `nums[k-1]`.

## 3. Brute-Force Complexity

Time `O(n log n)`, Space `O(1)`.

## 4. Bottleneck

We sort everything although only the top `k` matter.

## 5. Optimal Approach (and why it works)

Maintain a **min-heap of size `k`** holding the `k` largest elements seen. Push each value; if size exceeds `k`, pop the smallest. After processing all elements, the heap's top is the `k`-th largest.

*Alternative:* Quickselect: average `O(n)`, worst `O(n²)`.

**Complexity:** Heap: Time `O(n log k)`, Space `O(k)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;   // min-heap
    for (int x : nums) {
        pq.push(x);
        if ((int)pq.size() > k) pq.pop();                // evict the smallest
    }
    return pq.top();
}
```

## 7. Dry Run

`[3,2,1,5,6,4]`, `k = 2`

- push 3, 2 → `{2,3}`
- push 1 → size 3 → pop 1 → `{2,3}`
- push 5 → pop 2 → `{3,5}`
- push 6 → pop 3 → `{5,6}`
- push 4 → pop 4 → `{5,6}`
- top = `5`

## 8. Edge Cases

- `k = 1` → maximum
- `k = n` → minimum
- Duplicates `[3,2,3,1,2,4,5,5,6]`, `k=4` → `4`
- Single element
- Negative numbers

## Likely Follow-ups

- Why a *min*-heap for the largest? → The root is the smallest of the kept `k`, so the next candidate is compared against it.
- Quickselect trade-offs?
- `nth_element` in STL → average `O(n)`.
