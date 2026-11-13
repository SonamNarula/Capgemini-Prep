# Merge Two Sorted Lists

| | |
|---|---|
| **Section** | 5. Linked Lists |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Dummy head |
| **Problem** | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Merge two sorted linked lists into one sorted list by splicing the existing nodes.

**Clarify:** Ascending? One or both lists may be empty? Stability for equal values?

## 2. Brute Force

Dump both lists into an array, sort, rebuild.

## 3. Brute-Force Complexity

Time `O((n+m) log(n+m))`, Space `O(n+m)`.

## 4. Bottleneck

Both lists are already sorted; a single merge pass suffices, and no new nodes are needed.

## 5. Optimal Approach (and why it works)

Use a **dummy** node so the head needs no special case. Compare the two heads, attach the smaller to `tail`, advance. When one list ends, attach the remainder of the other.

**Complexity:** Time `O(n + m)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x = 0, ListNode* n = nullptr) : val(x), next(n) {}
};


ListNode* mergeTwoLists(ListNode* a, ListNode* b) {
    ListNode dummy(0);
    ListNode* tail = &dummy;
    while (a && b) {
        if (a->val <= b->val) { tail->next = a; a = a->next; }
        else                  { tail->next = b; b = b->next; }
        tail = tail->next;
    }
    tail->next = a ? a : b;          // attach whichever list remains
    return dummy.next;
}
```

## 7. Dry Run

`a = 1→3→5`, `b = 2→4`

- `1 ≤ 2` → take 1; `3 > 2` → take 2; `3 ≤ 4` → take 3; `5 > 4` → take 4
- `b` exhausted → attach `5`
- Result `1→2→3→4→5`

## 8. Edge Cases

- Both empty → `nullptr`
- One empty → return the other
- Equal values (`<=` keeps `a`'s nodes first → stable)
- Lists of very different lengths

## Likely Follow-ups

- Merge K sorted lists? → Min-heap of heads, `O(N log K)`.
- Recursive solution? → Elegant but `O(n+m)` stack.
