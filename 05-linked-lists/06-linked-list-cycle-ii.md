# Linked List Cycle II

| | |
|---|---|
| **Section** | 5. Linked Lists |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Find cycle entry |
| **Problem** | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the node where the cycle begins, or `nullptr` if there's none.

**Clarify:** May I modify the list? Constant extra space expected?

## 2. Brute Force

Store visited nodes in a set; the first repeated node is the cycle entry.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

The set costs `O(n)` memory. Floyd's algorithm has a neat property that locates the entry with pointers only.

## 5. Optimal Approach (and why it works)

Phase 1: run slow/fast until they meet inside the cycle. Phase 2: reset `slow` to `head`; move `slow` and `fast` **one step at a time**; they meet exactly at the cycle entry.

**Why:** Let `a` = distance head→entry, `L` = cycle length, `x` = entry→meeting point. At the meeting, `2(a + x) = a + x + kL` ⇒ `a = kL - x`. So walking `a` steps from the head and `a` steps from the meeting point (which is `L - x` before the entry, modulo the cycle) lands both on the entry.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x = 0, ListNode* n = nullptr) : val(x), next(n) {}
};


ListNode* detectCycle(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {                  // phase 1: meeting point found
            slow = head;                     // phase 2
            while (slow != fast) {
                slow = slow->next;
                fast = fast->next;
            }
            return slow;                     // cycle entry
        }
    }
    return nullptr;
}
```

## 7. Dry Run

`3 → 2 → 0 → -4 → (back to 2)`

- Phase 1: meet at node `-4`
- Phase 2: `slow=3`, `fast=-4`; step both → `slow=2`, `fast=2` → equal
- Return node `2`

## 8. Edge Cases

- No cycle → `nullptr`
- Cycle begins at the head
- Self-loop single node
- Cycle covers the whole list
- Two-node cycle

## Likely Follow-ups

- Count cycle length? → From the meeting node, loop until back to it.
- Why do they meet at the entry? → `a = kL - x` argument above.
