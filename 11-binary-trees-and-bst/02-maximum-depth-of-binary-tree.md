# Maximum Depth of Binary Tree

| | |
|---|---|
| **Section** | 11. Binary Trees & BST |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | DFS |
| **Problem** | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return the number of nodes on the longest root-to-leaf path.

**Clarify:** Depth counted in nodes or edges? Empty tree → `0`?

## 2. Brute Force

Enumerate every root-to-leaf path and take the longest.

## 3. Brute-Force Complexity

Time `O(n)`, Space `O(n)`–`O(h)`.

## 4. Bottleneck

No bottleneck; the recursion is already the simplest form.

## 5. Optimal Approach (and why it works)

`depth(node) = 1 + max(depth(left), depth(right))`; `depth(nullptr) = 0`.

**Complexity:** Time `O(n)`, Space `O(h)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x = 0, TreeNode* l = nullptr, TreeNode* r = nullptr) : val(x), left(l), right(r) {}
};


int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
// Iterative BFS alternative: count levels in a queue-based level-order traversal.
```

## 7. Dry Run

```
    3
   / \
  9  20
     / \
    15  7
```

- `depth(9)=1`, `depth(15)=1`, `depth(7)=1`
- `depth(20) = 1 + 1 = 2`
- `depth(3) = 1 + max(1, 2) = 3`

## 8. Edge Cases

- Empty tree → `0`
- Single node → `1`
- Skewed tree → `n`
- Only one child on a node

## Likely Follow-ups

- Minimum depth (LC 111)? → Careful with nodes having only one child.
- Balanced tree check?
