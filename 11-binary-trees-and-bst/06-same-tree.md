# Same Tree

| | |
|---|---|
| **Section** | 11. Binary Trees & BST |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | Recursion |
| **Problem** | [Same Tree](https://leetcode.com/problems/same-tree/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Check whether two binary trees have identical structure and node values.

**Clarify:** Is structure part of equality? (Yes.)

## 2. Brute Force

Serialise both trees (e.g. preorder with null markers) and compare strings.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

Extra memory; a simultaneous traversal compares directly.

## 5. Optimal Approach (and why it works)

Recurse on both trees together. Both null → `true`; exactly one null → `false`; values differ → `false`; else both subtree pairs must match.

**Complexity:** Time `O(n)`, Space `O(h)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x = 0, TreeNode* l = nullptr, TreeNode* r = nullptr) : val(x), left(l), right(r) {}
};


bool isSameTree(TreeNode* p, TreeNode* q) {
    if (!p && !q) return true;
    if (!p || !q) return false;
    if (p->val != q->val) return false;
    return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
}
```

## 7. Dry Run

`p = [1,2,3]`, `q = [1,2,3]` → all positions match → `true`

`p = [1,2]`, `q = [1,null,2]` → at `1`, `p->left=2` but `q->left=null` → `false`

## 8. Edge Cases

- Both empty → `true`
- One empty → `false`
- Same values, different structure → `false`
- Same structure, one value differs
- Skewed trees (deep recursion)

## Likely Follow-ups

- Subtree of another tree (LC 572)? → `isSameTree` at every node.
- Symmetric tree? → Compare `left` with mirrored `right`.
