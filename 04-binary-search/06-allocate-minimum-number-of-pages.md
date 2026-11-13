# Allocate Minimum Number of Pages

| | |
|---|---|
| **Section** | 4. Binary Search |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Binary search on answer |
| **Problem** | Allocate Minimum Number of Pages *(non-LeetCode / sheet-only problem)* |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

`n` books with `a[i]` pages (sorted order, as given). Allocate **contiguous** books to `m` students so that the **maximum pages** assigned to any student is minimised. Each student gets at least one book. Return that minimum, or `-1` if `m > n`.

**Clarify:** Must allocation be contiguous? (Yes.) Sorted by pages? (Not necessarily.)

## 2. Brute Force

Try all ways of splitting the array into `m` contiguous parts (exponential / `O(n^m)`), take the best maximum.

## 3. Brute-Force Complexity

Exponential in the worst case.

## 4. Bottleneck

The answer (max load) is monotonic: if load `L` works, any larger `L` works. So binary search `L` and check feasibility greedily.

## 5. Optimal Approach (and why it works)

Search range `lo = max(a)` (someone must hold the biggest book), `hi = sum(a)`. For a limit `L`, greedily fill each student until adding the next book would exceed `L`, then start a new student. If students needed `≤ m` → feasible (`hi = mid`), else `lo = mid + 1`.

**Complexity:** Time `O(n log(sum))`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
bool feasible(const vector<int>& a, int m, long long limit) {
    int students = 1;
    long long cur = 0;
    for (int x : a) {
        if (x > limit) return false;
        if (cur + x > limit) { students++; cur = x; }
        else cur += x;
    }
    return students <= m;
}

int findPages(vector<int>& a, int m) {
    int n = a.size();
    if (m > n) return -1;
    long long lo = *max_element(a.begin(), a.end());
    long long hi = accumulate(a.begin(), a.end(), 0LL);
    while (lo < hi) {
        long long mid = lo + (hi - lo) / 2;
        if (feasible(a, m, mid)) hi = mid;
        else lo = mid + 1;
    }
    return (int)lo;
}
```

## 7. Dry Run

`a = [12,34,67,90]`, `m = 2`

- `lo=90, hi=203`
- `mid=146` feasible → `hi=146`; `mid=118` feasible → `hi=118`
- `mid=104` needs 3 students → `lo=105`; `mid=111` needs 3 → `lo=112`
- `mid=115` feasible → `hi=115`; `mid=113` feasible (`[12,34,67] | [90]`) → `hi=113`
- `mid=112` needs 3 → `lo=113` → answer `113`

## 8. Edge Cases

- `m > n` → `-1`
- `m == n` → `max(a)`
- `m == 1` → `sum(a)`
- Single book
- Sum can exceed `int` → use `long long`

## Likely Follow-ups

- Same pattern: Split Array Largest Sum (LC 410), Painter's Partition, Capacity to Ship Packages.
- Why is `lo = max(a)` and not `1`? → Smaller limits can never hold the largest book.
