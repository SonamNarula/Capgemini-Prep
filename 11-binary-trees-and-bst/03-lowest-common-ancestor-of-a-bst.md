# Lowest Common Ancestor of a BST

| | |
|---|---|
| **Section** | 11. Binary Trees & BST |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | BST properties |
| **Problem** | [Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Find the lowest node in a BST that has both `p` and `q` as descendants (a node can be a descendant of itself).

**Clarify:** Both nodes exist? Unique values?

## 2. Brute Force

Find root-to-node paths for `p` and `q`, then compare the paths to find the last common node.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)` / `O(h)`.

## 4. Bottleneck

The BST ordering tells us which side each node is on without searching.

## 5. Optimal Approach (and why it works)

Start at the root. If both values are smaller, go left; if both larger, go right; otherwise the current node is the **split point**, which is the LCA.

**Complexity:** Time `O(h)`, Space `O(1)` (iterative).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x = 0, TreeNode* l = nullptr, TreeNode* r = nullptr) : val(x), left(l), right(r) {}
};


TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    while (root) {
        if (p->val < root->val && q->val < root->val) root = root->left;
        else if (p->val > root->val && q->val > root->val) root = root->right;
        else return root;                       // split point (or one of p/q)
    }
    return nullptr;
}
```

## 7. Dry Run

BST `[6,2,8,0,4,7,9,null,null,3,5]`

- `p=2, q=8`: at `6`, `2<6` and `8>6` → split → `6`
- `p=2, q=4`: at `6` both smaller → `2`; at `2`, `p==2`, `q=4>2` → split → `2` (a node is its own descendant)

## 8. Edge Cases

- `p` is ancestor of `q` → return `p`
- `p == q`
- Skewed BST (`O(n)`)
- Binary tree (not BST)? Use the general LCA recursion (LC 236)

## Likely Follow-ups

- General binary tree LCA? → Recursive post-order.
- Why `O(h)`? → One root-to-node path only.
