# Diameter of Binary Tree

| | |
|---|---|
| **Section** | 11. Binary Trees & BST |
| **Priority** | P1 |
| **Difficulty** | Easy |
| **Key pattern** | DFS |
| **Problem** | [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Length (in **edges**) of the longest path between any two nodes. The path may or may not pass through the root.

**Clarify:** Edges or nodes? (Edges.)

## 2. Brute Force

For each node compute the height of the left and right subtrees separately (`l + r`), repeating height calls at every node.

## 3. Brute-Force Complexity

Time `O(n²)` (skewed), Space `O(h)`.

## 4. Bottleneck

Heights are recomputed for every ancestor. One post-order pass can compute heights and update the diameter at the same time.

## 5. Optimal Approach (and why it works)

Post-order `height(node)`. At each node the longest path *through it* is `leftHeight + rightHeight`; update a global `best`. Return `1 + max(l, r)` to the parent.

**Complexity:** Time `O(n)`, Space `O(h)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x = 0, TreeNode* l = nullptr, TreeNode* r = nullptr) : val(x), left(l), right(r) {}
};


class Solution {
    int best = 0;
    int height(TreeNode* node) {
        if (!node) return 0;
        int l = height(node->left);
        int r = height(node->right);
        best = max(best, l + r);                 // path through this node
        return 1 + max(l, r);
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        best = 0;
        height(root);
        return best;
    }
};
```

## 7. Dry Run

```
    1
   / \
  2   3
 / \
4   5
```

- `h(4)=1, h(5)=1` → at `2`: `1+1=2`, returns 2
- `h(3)=1`
- At `1`: `2+1 = 3` → best `3` (path `4-2-1-3`)

## 8. Edge Cases

- Empty tree → `0`
- Single node → `0`
- Skewed tree → `n-1`
- Diameter not passing through root (deep subtree)
- Reset the global `best` between test cases

## Likely Follow-ups

- Return the actual path?
- Binary Tree Maximum Path Sum (LC 124) uses the same structure.
