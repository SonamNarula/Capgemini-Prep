# Find Middle of Singly Linked List

| | |
|---|---|
| **Section** | 5. Linked Lists |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Fast/slow pointers |
| **Problem** | [Find Middle of Singly Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the middle node. If there are two middle nodes (even length), return the **second** one.

**Clarify:** For even lengths, first or second middle? Can the list be empty?

## 2. Brute Force

Count the nodes, then walk `length / 2` steps.

## 3. Brute-Force Complexity

Time `O(n)` (two passes), Space `O(1)`.

## 4. Bottleneck

Two passes; fast/slow does it in one.

## 5. Optimal Approach (and why it works)

`slow` moves 1 step, `fast` moves 2. When `fast` reaches the end, `slow` is at the middle. (For first-middle on even lists, loop while `fast->next && fast->next->next`.)

**Complexity:** Time `O(n)` (one pass), Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x = 0, ListNode* n = nullptr) : val(x), next(n) {}
};


ListNode* middleNode(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

## 7. Dry Run

`1→2→3→4→5` → `slow` lands on `3`.

`1→2→3→4` → `slow=1,fast=1` → `slow=2,fast=3` → `slow=3,fast=null` → returns `3` (second middle).

## 8. Edge Cases

- Single node → itself
- Two nodes → second node
- Empty list → `nullptr`
- Odd vs even length

## Likely Follow-ups

- First middle for even length? → Change the loop condition.
- Where else is this used? → Palindrome list, merge sort on lists, Reorder List.
