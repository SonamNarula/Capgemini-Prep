# Reverse Linked List

| | |
|---|---|
| **Section** | 5. Linked Lists |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Three pointers |
| **Problem** | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Reverse a singly linked list and return the new head.

**Clarify:** Iterative or recursive? In place? May the list be empty?

## 2. Brute Force

Copy values into an array, reverse it, write back (or build a new list).

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

Extra memory; we can re-point existing `next` pointers instead.

## 5. Optimal Approach (and why it works)

Walk the list with `prev`, `cur`, `nxt`. For each node: save `nxt = cur->next`, point `cur->next = prev`, advance `prev = cur`, `cur = nxt`. When `cur` is null, `prev` is the new head.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x = 0, ListNode* n = nullptr) : val(x), next(n) {}
};


ListNode* reverseList(ListNode* head) {
    ListNode *prev = nullptr, *cur = head;
    while (cur) {
        ListNode* nxt = cur->next;   // save before breaking the link
        cur->next = prev;            // reverse the pointer
        prev = cur;
        cur = nxt;
    }
    return prev;
}
```

## 7. Dry Run

`1 → 2 → 3 → null`

- `cur=1`: `1→null`, `prev=1`, `cur=2`
- `cur=2`: `2→1`, `prev=2`, `cur=3`
- `cur=3`: `3→2`, `prev=3`, `cur=null`
- Return `3 → 2 → 1 → null`

## 8. Edge Cases

- Empty list → `nullptr`
- Single node
- Two nodes
- Very long list (iterative avoids recursion-depth issues)

## Likely Follow-ups

- Recursive version? → `O(n)` stack: `head->next->next = head; head->next = nullptr;`.
- Reverse in groups of K (LC 25) or between positions (LC 92)?
