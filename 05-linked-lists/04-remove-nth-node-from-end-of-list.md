# Remove Nth Node From End of List

| | |
|---|---|
| **Section** | 5. Linked Lists |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Fast pointer `n` steps ahead |
| **Problem** | [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Remove the `n`-th node from the **end** of a singly linked list and return the head, in one pass.

**Clarify:** Is `n` always valid (`1 ≤ n ≤ length`)? Can the head itself be removed?

## 2. Brute Force

Pass 1: compute length `L`. Pass 2: walk to node `L - n - 1` and unlink the next node.

## 3. Brute-Force Complexity

Time `O(L)` (two passes), Space `O(1)`.

## 4. Bottleneck

Two passes. A fixed gap of `n` between two pointers locates the target in one pass.

## 5. Optimal Approach (and why it works)

Add a dummy node before `head` (handles removing the head). Advance `fast` by `n` steps. Then move `fast` and `slow` together until `fast->next` is null. `slow` now sits just before the node to remove.

**Complexity:** Time `O(L)` (one pass), Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x = 0, ListNode* n = nullptr) : val(x), next(n) {}
};


ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0, head);
    ListNode *fast = &dummy, *slow = &dummy;
    for (int i = 0; i < n; i++) fast = fast->next;      // create the gap
    while (fast->next) {
        fast = fast->next;
        slow = slow->next;
    }
    ListNode* del = slow->next;                          // node to remove
    slow->next = del->next;
    delete del;
    return dummy.next;
}
```

## 7. Dry Run

`1→2→3→4→5`, `n = 2`

- `fast` advances 2 from dummy → at node `2`
- Move both: `fast=3,slow=1`; `fast=4,slow=2`; `fast=5,slow=3`; `fast->next` null → stop
- Remove `slow->next = 4` → `1→2→3→5`

## 8. Edge Cases

- Remove the head (`n == length`) → dummy node saves us
- Single node list, `n=1` → returns `nullptr`
- Remove the tail (`n=1`)
- `n` greater than length (if not guaranteed, check for null while advancing `fast`)

## Likely Follow-ups

- Why a dummy node? → Uniform handling when the head is deleted.
- Is `delete` always right? → Only if nodes were heap-allocated by you; on LeetCode it's fine.
