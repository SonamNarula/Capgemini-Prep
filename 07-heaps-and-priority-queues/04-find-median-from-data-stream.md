# Find Median from Data Stream

| | |
|---|---|
| **Section** | 7. Heaps & Priority Queues |
| **Priority** | P1 |
| **Difficulty** | Hard |
| **Key pattern** | Two heaps |
| **Problem** | [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Design a structure with `addNum(int)` and `findMedian()` for a stream of integers.

**Clarify:** Expected call frequency? Is `findMedian` called on non-empty data only? Numeric range?

## 2. Brute Force

Store all numbers in a vector; sort on every `findMedian`, or insert into a sorted position.

## 3. Brute-Force Complexity

`addNum` `O(n)` (sorted insert) or `findMedian` `O(n log n)` (sort each time).

## 4. Bottleneck

Maintaining a fully sorted order is more than needed. We only need the **middle** one or two elements.

## 5. Optimal Approach (and why it works)

Split the data into a lower half (**max-heap** `lo`) and an upper half (**min-heap** `hi`). Maintain: every element in `lo` ≤ every element in `hi`, and `lo.size() == hi.size()` or `lo.size() == hi.size() + 1`. To add `x`: push to `lo`, move `lo.top()` to `hi` (keeps ordering), then if `hi` is larger, move its top back to `lo`. Median is `lo.top()` if sizes differ, else average of both tops.

**Complexity:** `addNum` `O(log n)`, `findMedian` `O(1)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
class MedianFinder {
    priority_queue<int> lo;                                   // max-heap: smaller half
    priority_queue<int, vector<int>, greater<int>> hi;        // min-heap: larger half
public:
    void addNum(int num) {
        lo.push(num);
        hi.push(lo.top()); lo.pop();                          // keep lo <= hi
        if (hi.size() > lo.size()) {                          // rebalance sizes
            lo.push(hi.top()); hi.pop();
        }
    }
    double findMedian() {
        if (lo.size() > hi.size()) return lo.top();
        return ((double)lo.top() + hi.top()) / 2.0;
    }
};
```

## 7. Dry Run

Stream `1, 2, 3`

- add 1: `lo={1}`, `hi={}` → median `1`
- add 2: `lo={1}`, `hi={2}` → median `(1+2)/2 = 1.5`
- add 3: push 3 to `lo` → top 3 moves to `hi` (`hi={2,3}`), then `hi` larger → move `2` back: `lo={1,2}`, `hi={3}` → median `2`

## 8. Edge Cases

- Single number
- Two numbers (even count → average)
- Duplicates
- Negative numbers
- `double` conversion before adding to avoid `int` overflow

## Likely Follow-ups

- Most numbers in `[0,100]`? → Counting array and walk cumulative counts.
- Sliding-window median? → Two multisets or heaps with lazy deletion.
- Why are the two invariants needed? → They guarantee the median is always at the heap tops.
