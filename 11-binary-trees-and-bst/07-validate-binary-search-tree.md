# Validate Binary Search Tree

| | |
|---|---|
| **Section** | 11. Binary Trees & BST |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Range validation |
| **Problem** | [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Check whether a binary tree is a valid BST: for every node, **all** left-subtree values are smaller and **all** right-subtree values are larger (strict).

**Clarify:** Duplicates allowed? (No in LC.)

## 2. Brute Force

For each node, scan its whole left and right subtrees to verify min/max constraints.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(h)`.

## 4. Bottleneck

A common wrong shortcut checks only `left->val < node->val < right->val`, which misses deep violations. Passing allowed ranges down the recursion fixes it in one pass.

## 5. Optimal Approach (and why it works)

Recursively carry `(lo, hi)` bounds. A node must satisfy `lo < val < hi`. Going left tightens `hi = node->val`; going right tightens `lo = node->val`. Use `long long` bounds because node values can be `INT_MIN`/`INT_MAX`.

*Alternative:* inorder traversal must be strictly increasing.

**Complexity:** Time `O(n)`, Space `O(h)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x = 0, TreeNode* l = nullptr, TreeNode* r = nullptr) : val(x), left(l), right(r) {}
};


bool validate(TreeNode* node, long long lo, long long hi) {
    if (!node) return true;
    if (node->val <= lo || node->val >= hi) return false;
    return validate(node->left, lo, node->val) &&
           validate(node->right, node->val, hi);
}

bool isValidBST(TreeNode* root) {
    return validate(root, LLONG_MIN, LLONG_MAX);
}
```

## 7. Dry Run

```
    5
   / \
  1   4
     / \
    3   6
```

- Root `5`: range `(-inf, inf)` ✓
- Right child `4`: range `(5, inf)` → `4 <= 5` ✗ → `false`

The range `(5, inf)` is what catches this; a naive child-only check would miss deeper violations.

## 8. Edge Cases

- Empty tree → `true`
- Single node
- Node values `INT_MIN` / `INT_MAX` (hence `long long` bounds)
- Duplicates `[2,2,2]` → `false`
- Deep violation: `[5,4,6,null,null,3,7]` → `3` is in the right subtree of `5` → `false`

## Likely Follow-ups

- Why is the child-only check wrong? → Misses grandchildren violations.
- Inorder approach and its space trade-off?
- Recover a BST with two swapped nodes (LC 99)?
