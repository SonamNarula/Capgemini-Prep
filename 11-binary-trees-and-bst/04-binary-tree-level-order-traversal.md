# Binary Tree Level Order Traversal

| | |
|---|---|
| **Section** | 11. Binary Trees & BST |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | BFS |
| **Problem** | [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return node values level by level, left to right, as a list of lists.

**Clarify:** Empty tree → `[]`?

## 2. Brute Force

DFS with depth tracking, appending to `res[depth]` (also valid).

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(h)`.

## 4. Bottleneck

Nothing slow; BFS is the natural fit for level-wise grouping.

## 5. Optimal Approach (and why it works)

Queue-based BFS. At the start of each round the queue holds exactly one level: record `sz = q.size()`, pop `sz` nodes, collect their values, and push their children.

**Complexity:** Time `O(n)`, Space `O(w)` (max width).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x = 0, TreeNode* l = nullptr, TreeNode* r = nullptr) : val(x), left(l), right(r) {}
};


vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (!root) return res;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size();                       // nodes in this level
        vector<int> level;
        while (sz--) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left)  q.push(node->left);
            if (node->right) q.push(node->right);
        }
        res.push_back(level);
    }
    return res;
}
```

## 7. Dry Run

`[3,9,20,null,null,15,7]`

- Level 0: queue `[3]` → `[3]`, push `9,20`
- Level 1: `[9,20]` → push `15,7`
- Level 2: `[15,7]`
- Result `[[3],[9,20],[15,7]]`

## 8. Edge Cases

- Empty tree → `[]`
- Single node
- Skewed tree (one node per level)
- Perfect tree (last level has most nodes)

## Likely Follow-ups

- Zigzag order (LC 103)? → Reverse alternate levels.
- Right-side view (LC 199)? → Last node per level.
- Why capture `q.size()` before the inner loop? → The queue grows while iterating.
