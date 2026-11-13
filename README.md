# Capgemini-Prep

Interview preparation for the **Capgemini Exceller 2026-27 (13 LPA track)**: a C++ DSA problem set with a full written explanation for every question, plus subject-wise reference PDFs.

| | |
|---|---|
| **Technical assessment** | 18 November 2026 |
| **Technical interview** | 19 November 2026 |
| **Language** | C++ (C++17) |
| **Preparation window** | 13-17 November 2026 |

---

## What is in this repository

```
Capgemini-Prep/
├── Capgemini_13LPA_DSA_Master_Sheet.md          # prioritised problem checklist + 5-day schedule
├── 01-arrays-and-hashing/                        # one .md per problem (76 notes in total)
├── 02-two-pointers-and-sliding-window/
├── 03-strings-and-matrix/
├── 04-binary-search/
├── 05-linked-lists/
├── 06-stacks-and-queues/
├── 07-heaps-and-priority-queues/
├── 08-bit-manipulation-and-mathematics/
├── 09-recursion-and-backtracking/
├── 10-dynamic-programming/
├── 11-binary-trees-and-bst/
├── 12-graph-fundamentals/
├── Capgemini_Exceller_CPP_OOPs_Master_Bible.pdf
├── Capgemini_Exceller_OS_CN_Master_Bible.pdf
├── Capgemini_Exceller_SQL_DBMS_Master_Bible.pdf
└── Capgemini_Exceller_GenAI_RAG_Master_Bible.pdf
```

- **Master sheet:** the problem list, priorities (P0 / P1 / P2), STL checklist, complexity quick-reference and day-wise revision schedule.
- **Problem notes:** one file per problem, following the framework below.
- **Master Bibles (PDF):** reference material for C++ and OOPs, OS and Computer Networks, SQL and DBMS, and GenAI and RAG.

---

## How every problem note is structured

Each note follows the explanation framework from Section 13 of the master sheet, which is also the way to speak in the interview:

1. **Understand** the problem and constraints, with the clarifying questions worth asking
2. **Brute force** approach
3. **Brute-force complexity** (time and space)
4. **Bottleneck**: what makes brute force slow
5. **Optimal approach**: the pattern, why it works, and its complexity
6. **Clean C++ code** using the STL
7. **Dry run** on a worked example
8. **Edge cases**

Each note also ends with *Likely Follow-ups*, the questions an interviewer is most likely to ask next.

---

## Priority guide

| Priority | Meaning | Sections |
|---|---|---|
| **P0** | Highest priority | 1, 2, 4, 5 |
| **P1** | Important | 3, 6, 7, 8, 9, 10, 11 |
| **P2** | Additional coverage | 12 |

---

## Problem index

**Problem** column links to the note in this repo; **Link** column goes to the question itself (LeetCode / GeeksforGeeks).

### 1. Arrays & Hashing (P0)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Two Sum](01-arrays-and-hashing/01-two-sum.md) | Easy | `unordered_map`, single-pass lookup | [LeetCode](https://leetcode.com/problems/two-sum/) |
| [ ] | [Group Anagrams](01-arrays-and-hashing/02-group-anagrams.md) | Medium | Frequency array / sorted key | [LeetCode](https://leetcode.com/problems/group-anagrams/) |
| [ ] | [Top K Frequent Elements](01-arrays-and-hashing/03-top-k-frequent-elements.md) | Medium | Bucket sort / min-heap | [LeetCode](https://leetcode.com/problems/top-k-frequent-elements/) |
| [ ] | [Product of Array Except Self](01-arrays-and-hashing/04-product-of-array-except-self.md) | Medium | Prefix and suffix products | [LeetCode](https://leetcode.com/problems/product-of-array-except-self/) |
| [ ] | [Longest Consecutive Sequence](01-arrays-and-hashing/05-longest-consecutive-sequence.md) | Medium | `unordered_set`, sequence-start check | [LeetCode](https://leetcode.com/problems/longest-consecutive-sequence/) |
| [ ] | [Subarray Sum Equals K](01-arrays-and-hashing/06-subarray-sum-equals-k.md) | Medium | Prefix sum + frequency map | [LeetCode](https://leetcode.com/problems/subarray-sum-equals-k/) |
| [ ] | [Move All Zeros to End](01-arrays-and-hashing/07-move-all-zeros-to-end.md) | Easy | Two pointers, in-place | [LeetCode](https://leetcode.com/problems/move-zeroes/) |
| [ ] | [Find Equilibrium / Pivot Index](01-arrays-and-hashing/08-find-equilibrium-pivot-index.md) | Easy | Total sum minus left prefix | [LeetCode](https://leetcode.com/problems/find-pivot-index/) |
| [ ] | [Count Frequency of Array Elements](01-arrays-and-hashing/09-count-frequency-of-array-elements.md) | Easy | Hash map / in-place counting | [GFG](https://www.geeksforgeeks.org/problems/frequency-of-array-elements-1587115620/1) |

### 2. Two Pointers & Sliding Window (P0)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Valid Palindrome](02-two-pointers-and-sliding-window/01-valid-palindrome.md) | Easy | Left/right pointers | [LeetCode](https://leetcode.com/problems/valid-palindrome/) |
| [ ] | [3Sum](02-two-pointers-and-sliding-window/02-3sum.md) | Medium | Sorting + two pointers | [LeetCode](https://leetcode.com/problems/3sum/) |
| [ ] | [Container With Most Water](02-two-pointers-and-sliding-window/03-container-with-most-water.md) | Medium | Greedy two pointers | [LeetCode](https://leetcode.com/problems/container-with-most-water/) |
| [ ] | [Best Time to Buy and Sell Stock](02-two-pointers-and-sliding-window/04-best-time-to-buy-and-sell-stock.md) | Easy | Running minimum | [LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |
| [ ] | [Longest Substring Without Repeating Characters](02-two-pointers-and-sliding-window/05-longest-substring-without-repeating-characters.md) | Medium | Variable sliding window | [LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| [ ] | [Minimum Window Substring](02-two-pointers-and-sliding-window/06-minimum-window-substring.md) | Hard | Dynamic window + frequency | [LeetCode](https://leetcode.com/problems/minimum-window-substring/) |
| [ ] | [Max Sum Subarray of Distinct Elements of Size K](02-two-pointers-and-sliding-window/07-max-sum-subarray-of-distinct-elements-of-size-k.md) | Medium | Fixed window + frequency map | [LeetCode](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/) |
| [ ] | [Trapping Rain Water](02-two-pointers-and-sliding-window/08-trapping-rain-water.md) | Hard | Two pointers / leftMax and rightMax | [LeetCode](https://leetcode.com/problems/trapping-rain-water/) |

### 3. Strings & Matrix (P1)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Rotate Image](03-strings-and-matrix/01-rotate-image.md) | Medium | Transpose + reverse rows | [LeetCode](https://leetcode.com/problems/rotate-image/) |
| [ ] | [Spiral Matrix](03-strings-and-matrix/02-spiral-matrix.md) | Medium | Four boundaries | [LeetCode](https://leetcode.com/problems/spiral-matrix/) |
| [ ] | [Set Matrix Zeroes](03-strings-and-matrix/03-set-matrix-zeroes.md) | Medium | In-place markers | [LeetCode](https://leetcode.com/problems/set-matrix-zeroes/) |
| [ ] | [Encode and Decode Strings](03-strings-and-matrix/04-encode-and-decode-strings.md) | Medium | Length-prefixed encoding | [LeetCode (Premium)](https://leetcode.com/problems/encode-and-decode-strings/) |
| [ ] | [Run-Length String Compression](03-strings-and-matrix/05-run-length-string-compression.md) | Easy | Two pointers | [LeetCode](https://leetcode.com/problems/string-compression/) |
| [ ] | [Move All `#` Characters to Front](03-strings-and-matrix/06-move-all-hash-characters-to-front.md) | Easy | Single pass / backward overwrite | Original sheet only (no public link) |
| [ ] | [Check If One String Is Rotation of Another](03-strings-and-matrix/07-check-if-one-string-is-rotation-of-another.md) | Easy | `s1 + s1`, substring search | [LeetCode](https://leetcode.com/problems/rotate-string/) |

### 4. Binary Search (P0)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Binary Search](04-binary-search/01-binary-search.md) | Easy | Standard binary search | [LeetCode](https://leetcode.com/problems/binary-search/) |
| [ ] | [Search in Rotated Sorted Array](04-binary-search/02-search-in-rotated-sorted-array.md) | Medium | Identify sorted half | [LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/) |
| [ ] | [Find Minimum in Rotated Sorted Array](04-binary-search/03-find-minimum-in-rotated-sorted-array.md) | Medium | Compare `mid` with `high` | [LeetCode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) |
| [ ] | [Koko Eating Bananas](04-binary-search/04-koko-eating-bananas.md) | Medium | Binary search on answer | [LeetCode](https://leetcode.com/problems/koko-eating-bananas/) |
| [ ] | [First and Last Occurrence in Sorted Array](04-binary-search/05-first-and-last-occurrence-in-sorted-array.md) | Easy | Lower/upper bound | [LeetCode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) |
| [ ] | [Allocate Minimum Number of Pages](04-binary-search/06-allocate-minimum-number-of-pages.md) | Medium | Binary search on answer | [GFG](https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1) |

### 5. Linked Lists (P0)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Reverse Linked List](05-linked-lists/01-reverse-linked-list.md) | Easy | Three pointers | [LeetCode](https://leetcode.com/problems/reverse-linked-list/) |
| [ ] | [Merge Two Sorted Lists](05-linked-lists/02-merge-two-sorted-lists.md) | Easy | Dummy head | [LeetCode](https://leetcode.com/problems/merge-two-sorted-lists/) |
| [ ] | [Linked List Cycle](05-linked-lists/03-linked-list-cycle.md) | Easy | Floyd's algorithm | [LeetCode](https://leetcode.com/problems/linked-list-cycle/) |
| [ ] | [Remove Nth Node From End of List](05-linked-lists/04-remove-nth-node-from-end-of-list.md) | Medium | Fast pointer `n` steps ahead | [LeetCode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) |
| [ ] | [Find Middle of Singly Linked List](05-linked-lists/05-find-middle-of-singly-linked-list.md) | Easy | Fast/slow pointers | [LeetCode](https://leetcode.com/problems/middle-of-the-linked-list/) |
| [ ] | [Linked List Cycle II](05-linked-lists/06-linked-list-cycle-ii.md) | Medium | Find cycle entry | [LeetCode](https://leetcode.com/problems/linked-list-cycle-ii/) |
| [ ] | [Reorder List](05-linked-lists/07-reorder-list.md) | Medium | Middle + reverse + merge | [LeetCode](https://leetcode.com/problems/reorder-list/) |

### 6. Stacks & Queues (P1)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Valid Parentheses](06-stacks-and-queues/01-valid-parentheses.md) | Easy | Stack | [LeetCode](https://leetcode.com/problems/valid-parentheses/) |
| [ ] | [Daily Temperatures](06-stacks-and-queues/02-daily-temperatures.md) | Medium | Monotonic stack | [LeetCode](https://leetcode.com/problems/daily-temperatures/) |
| [ ] | [Next Greater Element to Right](06-stacks-and-queues/03-next-greater-element-to-right.md) | Medium | Monotonic stack | [GFG](https://www.geeksforgeeks.org/problems/next-larger-element-1587115620/1) |
| [ ] | [Implement Stack Using Queues](06-stacks-and-queues/04-implement-stack-using-queues.md) | Easy | Queue | [LeetCode](https://leetcode.com/problems/implement-stack-using-queues/) |
| [ ] | [Implement Queue Using Stacks](06-stacks-and-queues/05-implement-queue-using-stacks.md) | Easy | Stack | [LeetCode](https://leetcode.com/problems/implement-queue-using-stacks/) |
| [ ] | [Min Stack](06-stacks-and-queues/06-min-stack.md) | Medium | Auxiliary stack | [LeetCode](https://leetcode.com/problems/min-stack/) |
| [ ] | [Evaluate Reverse Polish Notation](06-stacks-and-queues/07-evaluate-reverse-polish-notation.md) | Medium | Stack | [LeetCode](https://leetcode.com/problems/evaluate-reverse-polish-notation/) |

### 7. Heaps & Priority Queues (P1)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Kth Largest Element in an Array](07-heaps-and-priority-queues/01-kth-largest-element-in-an-array.md) | Medium | Min-heap of size K | [LeetCode](https://leetcode.com/problems/kth-largest-element-in-an-array/) |
| [ ] | [Last Stone Weight](07-heaps-and-priority-queues/02-last-stone-weight.md) | Easy | Max-heap | [LeetCode](https://leetcode.com/problems/last-stone-weight/) |
| [ ] | [K Closest Points to Origin](07-heaps-and-priority-queues/03-k-closest-points-to-origin.md) | Medium | Heap | [LeetCode](https://leetcode.com/problems/k-closest-points-to-origin/) |
| [ ] | [Find Median from Data Stream](07-heaps-and-priority-queues/04-find-median-from-data-stream.md) | Hard | Two heaps | [LeetCode](https://leetcode.com/problems/find-median-from-data-stream/) |
| [ ] | [Top K Frequent Elements](01-arrays-and-hashing/03-top-k-frequent-elements.md) | Medium | Frequency + heap (same note as Section 1; both solutions included) | [LeetCode](https://leetcode.com/problems/top-k-frequent-elements/) |

### 8. Bit Manipulation & Mathematics (P1)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Single Number](08-bit-manipulation-and-mathematics/01-single-number.md) | Easy | XOR | [LeetCode](https://leetcode.com/problems/single-number/) |
| [ ] | [Number of 1 Bits](08-bit-manipulation-and-mathematics/02-number-of-1-bits.md) | Easy | Brian Kernighan | [LeetCode](https://leetcode.com/problems/number-of-1-bits/) |
| [ ] | [Counting Bits](08-bit-manipulation-and-mathematics/03-counting-bits.md) | Easy | DP + bit manipulation | [LeetCode](https://leetcode.com/problems/counting-bits/) |
| [ ] | [Prime Number Check](08-bit-manipulation-and-mathematics/04-prime-number-check.md) | Easy | Divisors up to square root | [GFG](https://www.geeksforgeeks.org/problems/prime-number2314/1) |
| [ ] | [Sieve of Eratosthenes](08-bit-manipulation-and-mathematics/05-sieve-of-eratosthenes.md) | Medium | Boolean sieve | [LeetCode (Count Primes)](https://leetcode.com/problems/count-primes/) |

### 9. Recursion & Backtracking (P1)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Subsets](09-recursion-and-backtracking/01-subsets.md) | Medium | Include/exclude | [LeetCode](https://leetcode.com/problems/subsets/) |
| [ ] | [Combination Sum](09-recursion-and-backtracking/02-combination-sum.md) | Medium | Backtracking | [LeetCode](https://leetcode.com/problems/combination-sum/) |
| [ ] | [Permutations](09-recursion-and-backtracking/03-permutations.md) | Medium | Visited array | [LeetCode](https://leetcode.com/problems/permutations/) |
| [ ] | [Generate Parentheses](09-recursion-and-backtracking/04-generate-parentheses.md) | Medium | Constraint-based recursion | [LeetCode](https://leetcode.com/problems/generate-parentheses/) |
| [ ] | [Letter Combinations of a Phone Number](09-recursion-and-backtracking/05-letter-combinations-of-a-phone-number.md) | Medium | Recursive choices | [LeetCode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) |

### 10. Dynamic Programming (P1)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Climbing Stairs](10-dynamic-programming/01-climbing-stairs.md) | Easy | 1D DP | [LeetCode](https://leetcode.com/problems/climbing-stairs/) |
| [ ] | [House Robber](10-dynamic-programming/02-house-robber.md) | Medium | State transition | [LeetCode](https://leetcode.com/problems/house-robber/) |
| [ ] | [Maximum Subarray](10-dynamic-programming/03-maximum-subarray.md) | Medium | Kadane's algorithm | [LeetCode](https://leetcode.com/problems/maximum-subarray/) |
| [ ] | [Coin Change](10-dynamic-programming/04-coin-change.md) | Medium | Unbounded knapsack | [LeetCode](https://leetcode.com/problems/coin-change/) |
| [ ] | [Longest Increasing Subsequence](10-dynamic-programming/05-longest-increasing-subsequence.md) | Medium | DP / binary search | [LeetCode](https://leetcode.com/problems/longest-increasing-subsequence/) |

### 11. Binary Trees & BST (P1)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Invert Binary Tree](11-binary-trees-and-bst/01-invert-binary-tree.md) | Easy | DFS | [LeetCode](https://leetcode.com/problems/invert-binary-tree/) |
| [ ] | [Maximum Depth of Binary Tree](11-binary-trees-and-bst/02-maximum-depth-of-binary-tree.md) | Easy | DFS | [LeetCode](https://leetcode.com/problems/maximum-depth-of-binary-tree/) |
| [ ] | [Lowest Common Ancestor of a BST](11-binary-trees-and-bst/03-lowest-common-ancestor-of-a-bst.md) | Medium | BST properties | [LeetCode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) |
| [ ] | [Binary Tree Level Order Traversal](11-binary-trees-and-bst/04-binary-tree-level-order-traversal.md) | Medium | BFS | [LeetCode](https://leetcode.com/problems/binary-tree-level-order-traversal/) |
| [ ] | [Diameter of Binary Tree](11-binary-trees-and-bst/05-diameter-of-binary-tree.md) | Easy | DFS | [LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/) |
| [ ] | [Same Tree](11-binary-trees-and-bst/06-same-tree.md) | Easy | Recursion | [LeetCode](https://leetcode.com/problems/same-tree/) |
| [ ] | [Validate Binary Search Tree](11-binary-trees-and-bst/07-validate-binary-search-tree.md) | Medium | Range validation | [LeetCode](https://leetcode.com/problems/validate-binary-search-tree/) |

### 12. Graph Fundamentals (P2)

| Done | Problem | Difficulty | Pattern | Link |
|---|---|---|---|---|
| [ ] | [Graph representation](12-graph-fundamentals/01-graph-representation.md) | Foundational | Adjacency list / matrix | [GFG](https://www.geeksforgeeks.org/problems/print-adjacency-list-1587115620/1) |
| [ ] | [BFS traversal](12-graph-fundamentals/02-bfs-traversal.md) | Foundational | Queue | [GFG](https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1) |
| [ ] | [DFS traversal](12-graph-fundamentals/03-dfs-traversal.md) | Foundational | Recursion | [GFG](https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1) |
| [ ] | [Number of connected components](12-graph-fundamentals/04-number-of-connected-components.md) | Foundational | DFS / BFS | [LeetCode (Premium)](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) |
| [ ] | [Number of Islands](12-graph-fundamentals/05-number-of-islands.md) | Medium | Grid DFS / BFS | [LeetCode](https://leetcode.com/problems/number-of-islands/) |
| [ ] | [Cycle detection in an undirected graph](12-graph-fundamentals/06-cycle-detection-in-an-undirected-graph.md) | Foundational | DFS / BFS | [GFG](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1) |

---

## Revision schedule

| Date | Focus |
|---|---|
| 13 Nov | Arrays and hashing, prefix sums, binary search, DBMS revision, C++ STL |
| 14 Nov | Two pointers, sliding window, fast and slow pointers, linked lists, stacks and queues |
| 15 Nov | Heaps, recursion and backtracking, binary trees, binary search on answer, selected DP |
| 16 Nov | Graph fundamentals, arrays and hashing revision, C++ concepts, SQL and DBMS, GenAI fundamentals, project explanation |
| 17 Nov | One timed coding mock, HackerRank practice, pseudocode practice, templates and complexity revision, interview dry runs |
| 18 Nov | **Technical assessment** |
| 19 Nov | **Technical interview** |

The full checklist version is in [`Capgemini_13LPA_DSA_Master_Sheet.md`](Capgemini_13LPA_DSA_Master_Sheet.md).

---

## How to use this repository

1. Attempt the problem on your own first.
2. Say the framework out loud: brute force, bottleneck, optimal, then complexity.
3. Open the note and compare your approach, code and edge cases.
4. Re-do the problem from a blank file a day later.
5. Tick the problem off in the tables above.

Revise patterns rather than chasing problem count. The aim is to solve unfamiliar problems using patterns already learned.

---

## Notes on accuracy

- Every C++ solution compiles with `g++ -std=c++17`, and the trickier ones were run against their dry-run examples.
- Problems without a LeetCode link come from the original sheet. Where the exact statement was not specified (for example, *Run-Length String Compression* or *Move All `#` Characters to Front*), the note states the assumption in its **Clarify** section. Confirm the statement with the interviewer.
- **Link column:** where the original sheet had no exact link, the closest LeetCode or GFG equivalent is used (e.g. *Move Zeroes*, *Find Pivot Index*, *String Compression*, *Rotate String*, *Count Primes*). Statements can differ slightly from the note's **Clarify** section; the note is the reference.
- LeetCode links marked *Premium* need a subscription.
- The source sheet labels some questions as Capgemini PYQs. Those labels are kept as supplied and have **not** been independently verified.

---

## Resources

- [NeetCode 150](https://neetcode.io/practice)
- [LeetCode](https://leetcode.com/problemset/)
- [HackerRank](https://www.hackerrank.com/)
- [Pseudocode practice](https://pseudo-studio.in/pseudocode-practice-questions)
- [IndiaBix](https://www.indiabix.com/)

---

*Brute Force → Optimization → Implementation → Dry Run → Complexity Analysis.*
