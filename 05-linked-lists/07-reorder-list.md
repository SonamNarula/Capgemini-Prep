# Reorder List

| | |
|---|---|
| **Section** | 5. Linked Lists |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Middle + reverse + merge |
| **Problem** | [Reorder List](https://leetcode.com/problems/reorder-list/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Reorder `L0 → L1 → … → Ln` into `L0 → Ln → L1 → Ln-1 → L2 → …`, in place, without changing node values.

**Clarify:** Only pointer changes allowed (no value swaps)? In place?

## 2. Brute Force

Store nodes in a vector, then link them using two indexes (front, back).

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

The vector costs `O(n)`. The same pattern is achievable by combining three standard sub-steps.

## 5. Optimal Approach (and why it works)

1) Find the middle with slow/fast. 2) Cut the list after the middle and reverse the second half. 3) Interleave: take one node from the first half, one from the reversed second half, until the second half is consumed.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x = 0, ListNode* n = nullptr) : val(x), next(n) {}
};


void reorderList(ListNode* head) {
    if (!head || !head->next) return;

    // 1) find the middle (first middle for even length)
    ListNode *slow = head, *fast = head;
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }

    // 2) cut and reverse the second half
    ListNode* second = slow->next;
    slow->next = nullptr;
    ListNode* prev = nullptr;
    while (second) {
        ListNode* nxt = second->next;
        second->next = prev;
        prev = second;
        second = nxt;
    }

    // 3) interleave
    ListNode *a = head, *b = prev;
    while (b) {
        ListNode *an = a->next, *bn = b->next;
        a->next = b;
        b->next = an;
        a = an;
        b = bn;
    }
}
```

## 7. Dry Run

`1→2→3→4→5`

- Middle: `slow=3`; second half `4→5`, first half `1→2→3`
- Reverse second: `5→4`
- Merge: `1→5→2`, then `2→4→3`; `b` exhausted
- Result `1→5→2→4→3`

## 8. Edge Cases

- Empty / single node / two nodes → unchanged
- Even length `1→2→3→4` → `1→4→2→3`
- Odd length
- Forgetting to set `slow->next = nullptr` creates a cycle

## Likely Follow-ups

- Palindrome Linked List uses the same first two steps.
- Why first middle for even lists? → The first half stays one node longer or equal, so interleaving ends cleanly.
