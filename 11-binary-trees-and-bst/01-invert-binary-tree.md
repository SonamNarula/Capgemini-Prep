# Invert Binary Tree

| | |
|---|---|
| **Section** | 11. Binary Trees & BST |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | DFS |
| **Problem** | [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Mirror a binary tree by swapping the left and right child of every node.

**Clarify:** In place? Is the tree possibly empty?

## 2. Brute Force

Build a new mirrored tree (copy nodes).

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`.

## 4. Bottleneck

No time bottleneck; swapping pointers in place avoids the extra tree.

## 5. Optimal Approach (and why it works)

At each node swap `left` and `right`, then recurse into both children. Base case: `nullptr`.

**Complexity:** Time `O(n)`, Space `O(h)` recursion stack (`h` = height).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x = 0, TreeNode* l = nullptr, TreeNode* r = nullptr) : val(x), left(l), right(r) {}
};


TreeNode* invertTree(TreeNode* root) {
    if (!root) return nullptr;
    swap(root->left, root->right);
    invertTree(root->left);
    invertTree(root->right);
    return root;
}
```

## 7. Dry Run

```
    4          4
   / \   →   / \
  2   7      7   2
```

- swap children of `4` → `(7, 2)`
- recurse into `7` and `2` and swap their (null) children
- For deeper trees, every level is mirrored

## 8. Edge Cases

- Empty tree → `nullptr`
- Single node
- Skewed tree (recursion depth `n`; consider iterative BFS)
- Perfect tree

## Likely Follow-ups

- Iterative version? → Queue/stack, swap on pop.
- Check if a tree is symmetric (LC 101)?
