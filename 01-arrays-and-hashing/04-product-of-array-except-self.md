# Product of Array Except Self

| | |
|---|---|
| **Section** | 1. Arrays & Hashing |
| **Priority** | P0 |
| **Difficulty** | Medium |
| **Key pattern** | Prefix and suffix products |
| **Problem** | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Return `res` where `res[i]` is the product of all elements except `nums[i]`, **without using division**, in `O(n)`.

**Clarify:** Can the array contain zeros? Does the product fit in 32-bit? (Guaranteed to.)

## 2. Brute Force

For each `i`, multiply all other elements with a nested loop.

## 3. Brute-Force Complexity

Time `O(n²)`, Space `O(1)` extra.

## 4. Bottleneck

Each position recomputes products that neighbouring positions already computed. Division would fix it but is banned (and breaks with zeros).

## 5. Optimal Approach (and why it works)

`res[i] = (product of everything left of i) × (product of everything right of i)`. Pass 1 (left→right): store the running prefix product into `res[i]`. Pass 2 (right→left): multiply `res[i]` by a running suffix product. The output array is reused, so extra space is `O(1)`.

**Complexity:** Time `O(n)`, Space `O(1)` extra (output excluded).

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n, 1);

    int prefix = 1;
    for (int i = 0; i < n; i++) {            // res[i] = product of nums[0..i-1]
        res[i] = prefix;
        prefix *= nums[i];
    }
    int suffix = 1;
    for (int i = n - 1; i >= 0; i--) {       // multiply by product of nums[i+1..n-1]
        res[i] *= suffix;
        suffix *= nums[i];
    }
    return res;
}
```

## 7. Dry Run

`nums = [1, 2, 3, 4]`

- Prefix pass: `res = [1, 1, 2, 6]`
- Suffix pass (suffix starts 1): `i=3: 6×1=6`, suffix→4; `i=2: 2×4=8`, suffix→12; `i=1: 1×12=12`, suffix→24; `i=0: 1×24=24`
- Result `[24, 12, 8, 6]`

## 8. Edge Cases

- One zero: `[1,0,3]` → `[0,3,0]`
- Two or more zeros → all zeros
- Negative numbers
- Two elements `[a, b]` → `[b, a]`
- Why not division? Zeros make it undefined

## Likely Follow-ups

- Can you do it with prefix/suffix arrays first, then optimise? → Yes, `O(n)` space version, then collapse it.
- How would you handle overflow? → Use `long long` / modulo if asked.
