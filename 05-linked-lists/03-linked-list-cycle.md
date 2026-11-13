# Linked List Cycle

| | |
|---|---|
| **Section** | 5. Linked Lists |
| **Priority** | P0 |
| **Difficulty** | Easy |
| **Key pattern** | Floyd's algorithm |
| **Problem** | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Detect whether a linked list contains a cycle.

**Clarify:** May I modify nodes? Can the list be empty?

## 2. Brute Force

Store visited node addresses in an `unordered_set`; if a node repeats, there's a cycle.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

The visited set uses `O(n)` memory. Two pointers at different speeds detect a cycle with no memory.

## 5. Optimal Approach (and why it works)

Floyd's tortoise and hare: `slow` moves 1 step, `fast` moves 2. If there's a cycle, `fast` eventually laps `slow` (the gap shrinks by 1 each step) and they meet. If `fast` hits `null`, there's no cycle.

**Complexity:** Time `O(n)`, Space `O(1)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x = 0, ListNode* n = nullptr) : val(x), next(n) {}
};


bool hasCycle(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

## 7. Dry Run

`3 → 2 → 0 → -4 → (back to 2)`

- `slow=3, fast=3`
- step 1: `slow=2, fast=0`
- step 2: `slow=0, fast=2`
- step 3: `slow=-4, fast=-4` → equal → `true`

## 8. Edge Cases

- Empty list → `false`
- Single node, no cycle
- Single node pointing to itself → `true`
- Cycle covering the whole list
- Two-node list

## Likely Follow-ups

- Find where the cycle starts? → Linked List Cycle II.
- Find the cycle length? → After meeting, count steps until you return to the meeting node.
