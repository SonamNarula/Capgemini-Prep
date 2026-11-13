# Arrays & Hashing — Capgemini 13 LPA Interview Master Notes

**Language:** C++17  
**Scope:** Arrays & Hashing fundamentals and high-value interview patterns  
**How to use:** Understand the brute-force idea, explain why it is slow, then explain and dry-run the optimal solution. Do not memorise code without understanding the invariant.

> Complexity convention: `n` = number of elements / characters; `k` = maximum string length; `m` = number of distinct values. Hash-table complexities are average-case unless stated otherwise.

## Question index

| Index | Problem | Main pattern | Priority |
|---:|---|---|---|
| 1 | Contains Duplicate | Hash set | Core |
| 2 | Valid Anagram | Frequency counting | Core |
| 3 | Two Sum | Complement + hash map | Core |
| 4 | Group Anagrams | Canonical key + hash map | Core |
| 5 | Top K Frequent Elements | Frequency map + bucket sort / heap | Core |
| 6 | Encode and Decode Strings | Length-prefix serialization | Optional |
| 7 | Product of Array Except Self | Prefix + suffix products | Core |
| 8 | Valid Sudoku | Row/column/box validation | Optional |
| 9 | Longest Consecutive Sequence | Hash set + sequence starts | Core |
| 10 | Subarray Sum Equals K | Prefix sum + frequency map | High-value extension |
| 11 | Find Pivot Index / Equilibrium Index | Total sum + running prefix | High-value extension |
| 12 | Move Zeroes | In-place two pointers | High-value extension |
| 13 | Frequency of Array Elements | Counting / hash map | High-value extension |

The first nine are the standard NeetCode 150 Arrays & Hashing set. The final four are useful adjacent array/hash-map patterns often encountered in company-specific sheets. Prioritise the Core and High-value extension rows; leave Optional problems until time permits.

---

# 1. Contains Duplicate

**Question:** Given an integer array `nums`, return `true` if any value appears at least twice; otherwise return `false`.

**Example:** `nums = [1, 2, 3, 1]` → `true`

## Brute force

Compare every pair of different indices. If any pair has equal values, a duplicate exists.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool containsDuplicateBrute(vector<int>& nums) {
    int n = nums.size();

    // Try every pair (i, j), with j strictly after i.
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] == nums[j]) {
                return true; // Found a repeated value.
            }
        }
    }

    return false; // No pair had equal values.
}
```

- **Time:** `O(n^2)` — there can be roughly `n(n-1)/2` comparisons.
- **Auxiliary space:** `O(1)`.

## Optimal: hash set

A set stores values already seen. Before inserting the current value, check whether it is already present.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> seen;

    for (int x : nums) {
        // If x is already stored, this occurrence is a duplicate.
        if (seen.count(x)) {
            return true;
        }

        // First time seeing x: remember it.
        seen.insert(x);
    }

    return false;
}
```

### Dry run

Input: `[1, 2, 3, 1]`

| Current value | Set before check | Action |
|---:|---|---|
| 1 | `{}` | Not present; insert 1 |
| 2 | `{1}` | Not present; insert 2 |
| 3 | `{1, 2}` | Not present; insert 3 |
| 1 | `{1, 2, 3}` | Already present; return `true` |

- **Average time:** `O(n)` — expected `O(1)` set lookup/insertion per element.
- **Auxiliary space:** `O(n)` — in the worst case all values are distinct.

### Interview follow-ups

1. **Can you solve it without extra space?** Sort the array and compare adjacent elements. Time `O(n log n)`; auxiliary space depends on the sorting implementation.
2. **`set` vs `unordered_set`?** `set` is ordered and gives `O(log n)` operations; `unordered_set` is hash-based and gives average `O(1)` operations.
3. **Worst-case hash lookup?** `O(n)` in a pathological collision case; average-case is `O(1)`.

---

# 2. Valid Anagram

**Question:** Given strings `s` and `t`, return `true` if `t` is an anagram of `s`. Anagrams contain exactly the same characters with the same frequencies.

**Example:** `s = "anagram"`, `t = "nagaram"` → `true`

## Brute force: sorting

If both strings are sorted, equal character multisets produce equal sorted strings.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isAnagramBrute(string s, string t) {
    if (s.size() != t.size()) {
        return false; // Different lengths cannot be anagrams.
    }

    sort(s.begin(), s.end());
    sort(t.begin(), t.end());

    return s == t;
}
```

- **Time:** `O(n log n)`.
- **Auxiliary space:** implementation-dependent; sorting may use stack space.

## Optimal for lowercase English letters: frequency array

Count each letter in `s`, subtract each letter in `t`. All counts must end at zero.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isAnagram(string s, string t) {
    if (s.size() != t.size()) {
        return false;
    }

    // One slot for each lowercase English letter.
    int freq[26] = {0};

    for (int i = 0; i < (int)s.size(); i++) {
        freq[s[i] - 'a']++; // Character from s contributes +1.
        freq[t[i] - 'a']--; // Character from t contributes -1.
    }

    // Anagrams have exactly balanced frequencies.
    for (int count : freq) {
        if (count != 0) {
            return false;
        }
    }

    return true;
}
```

### Dry run

`s = "abca"`, `t = "aacb"`

- Read `s[0]='a'`, `t[0]='a'`: `a` gets `+1 - 1 = 0`.
- Read `s[1]='b'`, `t[1]='a'`: `b = +1`, `a = -1`.
- Read `s[2]='c'`, `t[2]='c'`: `c` returns to zero.
- Read `s[3]='a'`, `t[3]='b'`: `a` returns to zero, `b` returns to zero.
- Every count is zero → `true`.

- **Time:** `O(n + 26)` = `O(n)`.
- **Auxiliary space:** `O(1)` because the array always has 26 entries.

### Interview follow-ups

1. **What if uppercase letters are allowed?** Use a 128/256-sized array for an agreed ASCII character set, or normalise case if the problem says case-insensitive.
2. **What if arbitrary Unicode is allowed?** Use a suitable Unicode-aware frequency map; `char` in C++ is not a full Unicode code point.
3. **Why check lengths first?** Anagrams must have equal character counts.

---

# 3. Two Sum

**Question:** Given `nums` and `target`, return the indices of two different elements whose sum equals `target`. Assume exactly one solution if the problem states that guarantee.

**Example:** `nums = [2, 7, 11, 15]`, `target = 9` → `[0, 1]`

## Brute force

Try all pairs `i < j`.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> twoSumBrute(vector<int>& nums, int target) {
    int n = nums.size();

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                return {i, j};
            }
        }
    }

    return {}; // No pair found.
}
```

- **Time:** `O(n^2)`.
- **Auxiliary space:** `O(1)`.

## Optimal: one-pass hash map

For current value `x`, the required partner is `target - x`. Store each earlier value with its index.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> twoSum(vector<int>& nums, int target) {
    // Maps a value to an index where it appeared earlier.
    unordered_map<int, int> indexOf;

    for (int i = 0; i < (int)nums.size(); i++) {
        int need = target - nums[i];

        // Search before inserting current value, so we never use
        // the same array element twice.
        auto it = indexOf.find(need);
        if (it != indexOf.end()) {
            return {it->second, i};
        }

        // Remember current value for a future element.
        indexOf[nums[i]] = i;
    }

    return {};
}
```

### Dry run

`nums = [2, 7, 11, 15]`, `target = 9`

| Index | Value | Needed | Map before lookup | Result |
|---:|---:|---:|---|---|
| 0 | 2 | 7 | `{}` | Not found; store `2 → 0` |
| 1 | 7 | 2 | `{2 → 0}` | Found; return `[0, 1]` |

- **Average time:** `O(n)`.
- **Auxiliary space:** `O(n)`.

### Interview follow-ups

1. **Why search before insertion?** Prevents using the same index twice; `[3]`, target `6` must not return index 0 twice.
2. **What about duplicates?** `[3, 3]`, target `6`: first 3 is stored, second finds it.
3. **Can two pointers solve it?** Yes, if sorted, but sorting changes positions. Preserve original indices by sorting `(value, originalIndex)` pairs.
4. **What if there are many answers?** Continue scanning / store all relevant indices according to the required output contract.

---

# 4. Group Anagrams

**Question:** Group strings that are anagrams of one another.

**Example:** `["eat","tea","tan","ate","nat","bat"]` → groups equivalent to `[["eat","tea","ate"],["tan","nat"],["bat"]]`. Group order does not matter.

## Brute-force idea

For each string, compare it with existing group representatives by sorting both strings. Put it into a matching group or create a new group.

- If every string is compared against up to `n` representatives and sorting costs `O(k log k)`, time can be `O(n^2 k log k)`.
- Group storage is `O(nk)` for the input strings / output.

## Better: sorted string as a canonical key

All anagrams have the same sorted characters. Use that sorted string as the hash-map key.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<vector<string>> groupAnagrams(vector<string>& strs) {
    // Key = sorted version of a word; value = words with that key.
    unordered_map<string, vector<string>> groups;

    for (const string& word : strs) {
        string key = word;

        // Every anagram produces the same key.
        sort(key.begin(), key.end());

        groups[key].push_back(word);
    }

    vector<vector<string>> answer;

    // Move each grouped collection into the final answer.
    for (auto& entry : groups) {
        answer.push_back(move(entry.second));
    }

    return answer;
}
```

### Dry run

Input: `["eat", "tea", "tan", "ate", "nat", "bat"]`

| Word | Sorted key | Group |
|---|---|---|
| eat | aet | 1 |
| tea | aet | 1 |
| tan | ant | 2 |
| ate | aet | 1 |
| nat | ant | 2 |
| bat | abt | 3 |

- **Time:** `O(n * k log k)` for `n` words of maximum length `k`.
- **Space:** `O(nk)` for stored words and keys, excluding / including output according to the accounting convention.

### Alternative: 26-count signature

For lowercase English letters, count each letter and use that frequency vector as the key. This avoids sorting each word; expected time becomes `O(nk)` with a suitable custom hash for the array key.

### Interview follow-ups

1. **Why does sorting work?** Anagrams differ only in order; sorting removes order.
2. **Can sorting be avoided?** Yes, use a 26-letter frequency signature for lowercase English letters.
3. **Does unordered-map iteration preserve group order?** No. The problem normally permits any group order.

---

# 5. Top K Frequent Elements

**Question:** Return the `k` elements with the highest frequencies.

**Example:** `nums = [1,1,1,2,2,3]`, `k = 2` → `[1,2]`.

## Brute force / straightforward approach: map + sort

Count frequencies, then sort distinct `(value, frequency)` pairs by frequency.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> topKFrequentSort(vector<int>& nums, int k) {
    unordered_map<int, int> freq;

    for (int x : nums) {
        freq[x]++;
    }

    vector<pair<int, int>> items; // {frequency, value}
    for (auto& entry : freq) {
        items.push_back({entry.second, entry.first});
    }

    // Highest frequency first.
    sort(items.begin(), items.end(),
         [](const auto& a, const auto& b) {
             return a.first > b.first;
         });

    vector<int> answer;
    for (int i = 0; i < k && i < (int)items.size(); i++) {
        answer.push_back(items[i].second);
    }

    return answer;
}
```

- **Time:** `O(n + m log m)`, where `m` is the number of distinct values.
- **Space:** `O(m)`.

## Optimal expected linear approach: bucket sort

A value can appear at most `n` times. Make bucket `f` contain all values whose frequency is `f`; scan buckets from high to low.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> topKFrequent(vector<int>& nums, int k) {
    unordered_map<int, int> freq;

    // Step 1: Count each value.
    for (int x : nums) {
        freq[x]++;
    }

    // Step 2: Bucket index represents frequency.
    // No element can have frequency greater than nums.size().
    vector<vector<int>> bucket(nums.size() + 1);

    for (auto& entry : freq) {
        int value = entry.first;
        int count = entry.second;
        bucket[count].push_back(value);
    }

    // Step 3: Read highest-frequency buckets first.
    vector<int> answer;

    for (int f = (int)nums.size(); f >= 1; f--) {
        for (int value : bucket[f]) {
            answer.push_back(value);

            if ((int)answer.size() == k) {
                return answer;
            }
        }
    }

    return answer;
}
```

### Dry run

`[1,1,1,2,2,3]`, `k=2`

- Frequency map: `1 → 3`, `2 → 2`, `3 → 1`.
- Bucket 3: `[1]`.
- Bucket 2: `[2]`.
- Bucket 1: `[3]`.
- Scan downwards: take `1`, then `2`; stop at two answers.

- **Time:** `O(n + m)` = `O(n)`.
- **Space:** `O(n)`.

### Alternative: min heap

Keep a min heap of at most `k` elements ordered by frequency. Time `O(n + m log k)`; space `O(m + k)` including frequency map.

### Interview follow-ups

1. **Why is bucket sort linear here?** Frequencies are integers bounded by `n`, so there are only `n+1` buckets.
2. **What if several values tie at the cutoff?** Follow the problem's tie rule; if any tied answer is acceptable, any valid set works.
3. **When is a heap preferable?** When `k` is small relative to the number of distinct elements or when maintaining top-k dynamically.

---

# 6. Encode and Decode Strings

**Question:** Design `encode` to turn a list of strings into one string and `decode` to reconstruct the exact original list. Strings may contain delimiters and may be empty.

## Why a simple delimiter is unsafe

Joining with `#` is ambiguous if an input string itself contains `#`. For example, `["a#b", "c"]` cannot safely be split on every `#` without escaping rules.

## Length-prefix encoding

Write each string as: `length + '#' + contents`. The decoder reads the length, then consumes exactly that many characters.

```cpp
#include <bits/stdc++.h>
using namespace std;

class Codec {
public:
    string encode(const vector<string>& strs) {
        string encoded;

        for (const string& word : strs) {
            // Store the number of characters, then a separator.
            encoded += to_string(word.size());
            encoded += '#';

            // Append exactly the original word, even if it has '#'.
            encoded += word;
        }

        return encoded;
    }

    vector<string> decode(const string& encoded) {
        vector<string> result;
        int i = 0;
        int n = encoded.size();

        while (i < n) {
            // Find the separator after the decimal length.
            int j = i;
            while (j < n && encoded[j] != '#') {
                j++;
            }

            // Convert the length prefix to an integer.
            int length = stoi(encoded.substr(i, j - i));

            // The next 'length' characters belong to this word.
            string word = encoded.substr(j + 1, length);
            result.push_back(word);

            // Move to the first character after the decoded word.
            i = j + 1 + length;
        }

        return result;
    }
};
```

### Dry run

Input: `["hi#there", "", "cat"]`

Encoded string: `8#hi#there0#3#cat`

1. Read `8#`, consume exactly `hi#there`.
2. Read `0#`, consume zero characters → empty string.
3. Read `3#`, consume `cat`.
4. Reconstructed list is exactly `["hi#there", "", "cat"]`.

- **Encode time:** `O(total input characters + number of strings)`.
- **Decode time:** `O(encoded length)` for this implementation.
- **Space:** `O(encoded length)` for encoded / decoded data; output storage is required.

### Interview follow-ups

1. **Why is it unambiguous?** The decoder knows the exact payload length; delimiter characters inside the payload are ordinary data.
2. **How are empty strings represented?** `0#`.
3. **What if the data is transmitted over a network?** Also specify character encoding, maximum lengths, malformed-input handling and framing.

---

# 7. Product of Array Except Self

**Question:** Return `answer[i]` equal to the product of every element in `nums` except `nums[i]`. Do not use division. The usual problem asks for `O(n)` time and constant extra space excluding the output array.

**Example:** `[1,2,3,4]` → `[24,12,8,6]`.

## Brute force

For every index, multiply all other positions.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> productExceptSelfBrute(vector<int>& nums) {
    int n = nums.size();
    vector<int> answer(n, 1);

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i != j) {
                answer[i] *= nums[j];
            }
        }
    }

    return answer;
}
```

- **Time:** `O(n^2)`.
- **Auxiliary space:** `O(1)` excluding output.

## Optimal: prefix and suffix products

For index `i`, multiply the product strictly to its left by the product strictly to its right. First pass stores left products; second pass multiplies in right products.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> answer(n, 1);

    // prefix = product of all elements strictly to the left of i.
    int prefix = 1;

    for (int i = 0; i < n; i++) {
        answer[i] = prefix; // Save left-side product for this index.
        prefix *= nums[i];  // Include nums[i] for the next index.
    }

    // suffix = product of all elements strictly to the right of i.
    int suffix = 1;

    for (int i = n - 1; i >= 0; i--) {
        answer[i] *= suffix; // Combine left and right products.
        suffix *= nums[i];   // Include nums[i] for the next index left.
    }

    return answer;
}
```

### Dry run

Input: `[1,2,3,4]`

After left-to-right pass, `answer = [1,1,2,6]`.

Right-to-left pass:

| Index | Current suffix before multiply | Answer after multiply |
|---:|---:|---:|
| 3 | 1 | 6 |
| 2 | 4 | 8 |
| 1 | 12 | 12 |
| 0 | 24 | 24 |

Final: `[24,12,8,6]`.

- **Time:** `O(n)`.
- **Auxiliary space:** `O(1)` excluding output.

### Edge cases

- One zero: only the zero's position can have a nonzero product.
- Two or more zeros: all answers are zero.
- Negative values: prefix/suffix multiplication still works.
- Consider integer overflow if constraints exceed the result type.

### Interview follow-ups

1. **Why no division?** Division is prohibited and fails straightforwardly with zeros.
2. **Why is extra space constant?** Only `prefix` and `suffix` scalars are used besides the required output.
3. **What is the invariant?** Before each multiplication, `prefix` / `suffix` contains the product strictly on that side.

---

# 8. Valid Sudoku

**Question:** Check whether a partially filled 9×9 Sudoku board violates any rule: no repeated digit in a row, column or 3×3 box. `'.'` means empty.

## Brute force

For each filled cell, scan the rest of its row, column and box for the same digit. On a fixed 9×9 board this is bounded work, but it repeats checks.

## Optimal clean implementation: boolean tracking

Use three groups of seen flags: row, column and box. Each digit can be represented by index `digit - '1'`.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isValidSudoku(vector<vector<char>>& board) {
    // rows[r][d] means digit d has already appeared in row r.
    bool rows[9][9] = {};
    bool cols[9][9] = {};
    bool boxes[9][9] = {};

    for (int r = 0; r < 9; r++) {
        for (int c = 0; c < 9; c++) {
            char ch = board[r][c];

            // Empty cells do not affect validity.
            if (ch == '.') {
                continue;
            }

            int digit = ch - '1'; // '1' -> 0, '9' -> 8.

            // Map (r, c) to one of the nine 3x3 boxes.
            int box = (r / 3) * 3 + (c / 3);

            // Any existing flag means this digit is repeated.
            if (rows[r][digit] || cols[c][digit] || boxes[box][digit]) {
                return false;
            }

            // Mark digit as seen in all three constraints.
            rows[r][digit] = true;
            cols[c][digit] = true;
            boxes[box][digit] = true;
        }
    }

    return true;
}
```

### Dry run

Suppose `'5'` is at `(0,0)`:

- `digit = '5' - '1' = 4`.
- `box = (0/3)*3 + (0/3) = 0`.
- Mark `rows[0][4]`, `cols[0][4]`, `boxes[0][4]`.

If another `'5'` appears at `(0,2)`, `rows[0][4]` is already true, so return `false`.

- **Time:** `O(81)` = `O(1)` for fixed 9×9 Sudoku.
- **Space:** `O(3 × 9 × 9)` = `O(1)` for fixed board size.

### Interview follow-ups

1. **Why the box formula?** Each 3×3 region has an index from 0 to 8: `(r/3)*3 + c/3`.
2. **Does valid mean solved?** No; empty cells are allowed. It only checks the filled digits for conflicts.
3. **Can sets be used?** Yes, but boolean arrays are simpler and have constant-time direct indexing.

---

# 9. Longest Consecutive Sequence

**Question:** Given an unsorted integer array, return the length of its longest run of consecutive values. Target expected `O(n)` time.

**Example:** `[100,4,200,1,3,2]` → `4`, from sequence `1,2,3,4`.

## Brute force

For each value, repeatedly scan the entire array to see whether the next value exists. This can take `O(n^2)`.

## Optimal: hash set and only start at sequence beginnings

Put all values into a set. A value `x` begins a sequence only if `x-1` is absent. Count forward from such beginnings.

```cpp
#include <bits/stdc++.h>
using namespace std;

int longestConsecutive(vector<int>& nums) {
    unordered_set<int> values(nums.begin(), nums.end());

    int longest = 0;

    for (int x : values) {
        // If x-1 exists, x is not the beginning of this sequence.
        if (values.count(x - 1)) {
            continue;
        }

        // x is a sequence start. Count x, x+1, x+2, ...
        int current = x;
        int length = 1;

        while (values.count(current + 1)) {
            current++;
            length++;
        }

        longest = max(longest, length);
    }

    return longest;
}
```

### Dry run

Input: `[100,4,200,1,3,2]`

- `100`: predecessor `99` absent → sequence length 1.
- `4`: predecessor `3` exists → skip.
- `200`: predecessor `199` absent → sequence length 1.
- `1`: predecessor `0` absent → count `1,2,3,4`, length 4.
- `2` and `3`: each has a predecessor, so skip.

Answer: `4`.

- **Expected time:** `O(n)` — each sequence is extended from its beginning; hash operations are average `O(1)`.
- **Space:** `O(n)` for the set.

### Interview follow-ups

1. **Why only start at a number with no predecessor?** It prevents repeatedly traversing the same consecutive run.
2. **What about duplicates?** The set removes duplicates automatically.
3. **Can sorting solve it?** Yes, `O(n log n)` time after handling duplicates; the set approach meets expected `O(n)`.
4. **Integer boundary caution?** If input may contain `INT_MIN` or `INT_MAX`, guard `x-1` and `current+1` against signed overflow (or use a wider integer type for the set).

---

# 10. Subarray Sum Equals K — high-value extension

**Question:** Count the number of contiguous subarrays whose sum equals `k`.

**Example:** `nums = [1,1,1]`, `k = 2` → `2` (`[1,1]` at indices 0–1 and 1–2).

## Brute force

Enumerate all start indices and extend the end index while maintaining the running sum.

```cpp
#include <bits/stdc++.h>
using namespace std;

int subarraySumBrute(vector<int>& nums, int k) {
    int n = nums.size();
    int answer = 0;

    for (int start = 0; start < n; start++) {
        int sum = 0;

        for (int end = start; end < n; end++) {
            sum += nums[end]; // Sum of subarray [start, end].

            if (sum == k) {
                answer++;
            }
        }
    }

    return answer;
}
```

- **Time:** `O(n^2)`.
- **Auxiliary space:** `O(1)`.

## Optimal: prefix sum + frequency map

Let `prefix` be the sum from index 0 through the current index. If an earlier prefix was `prefix - k`, the elements between that earlier prefix and the current position sum to `k`.

Store how many times each prefix sum has occurred. Initialise frequency of prefix sum zero to one to count subarrays beginning at index zero.

```cpp
#include <bits/stdc++.h>
using namespace std;

int subarraySum(vector<int>& nums, int k) {
    unordered_map<long long, int> prefixFrequency;

    // Empty prefix: one way to have sum 0 before reading elements.
    prefixFrequency[0] = 1;

    long long prefix = 0;
    int answer = 0;

    for (int x : nums) {
        prefix += x;

        // Need an earlier prefix equal to prefix - k.
        long long needed = prefix - k;

        if (prefixFrequency.count(needed)) {
            answer += prefixFrequency[needed];
        }

        // Record current prefix AFTER counting, so we only use
        // prefixes from positions strictly before the current end.
        prefixFrequency[prefix]++;
    }

    return answer;
}
```

### Dry run

`nums = [1,1,1]`, `k = 2`

Initial map: `{0:1}`.

| Current value | Prefix | Needed prefix | Matches | Answer | Map update |
|---:|---:|---:|---:|---:|---|
| 1 | 1 | -1 | 0 | 0 | `{0:1, 1:1}` |
| 1 | 2 | 0 | 1 | 1 | `{0:1, 1:1, 2:1}` |
| 1 | 3 | 1 | 1 | 2 | `{0:1, 1:1, 2:1, 3:1}` |

Answer: `2`.

- **Expected time:** `O(n)`.
- **Space:** `O(n)`.

### Interview follow-ups

1. **Why not sliding window?** Negative numbers break the monotonic property needed by the usual sum-based sliding window.
2. **Why initialise `{0:1}`?** It counts a valid subarray from index 0 when the current prefix itself equals `k`.
3. **Why store frequencies, not just whether a prefix exists?** The same prefix sum can occur multiple times, representing multiple valid starting positions.
4. **Does it work with negative numbers?** Yes; prefix sums plus a frequency map do not require nonnegative values.

---

# 11. Find Pivot Index / Equilibrium Index

**Question:** Find an index where the sum of elements strictly to its left equals the sum strictly to its right. Return `-1` if none exists.

**Example:** `[1,7,3,6,5,6]` → `3`, because left sum is `1+7+3=11` and right sum is `5+6=11`.

## Brute force

For each index, separately sum everything left and everything right.

```cpp
#include <bits/stdc++.h>
using namespace std;

int pivotIndexBrute(vector<int>& nums) {
    int n = nums.size();

    for (int i = 0; i < n; i++) {
        int leftSum = 0;
        int rightSum = 0;

        for (int j = 0; j < i; j++) {
            leftSum += nums[j];
        }

        for (int j = i + 1; j < n; j++) {
            rightSum += nums[j];
        }

        if (leftSum == rightSum) {
            return i;
        }
    }

    return -1;
}
```

- **Time:** `O(n^2)`.
- **Auxiliary space:** `O(1)`.

## Optimal: total sum + running left sum

At index `i`, right sum equals `totalSum - leftSum - nums[i]`. Compare it to `leftSum`.

```cpp
#include <bits/stdc++.h>
using namespace std;

int pivotIndex(vector<int>& nums) {
    long long totalSum = 0;

    // Calculate sum of the entire array once.
    for (int x : nums) {
        totalSum += x;
    }

    long long leftSum = 0;

    for (int i = 0; i < (int)nums.size(); i++) {
        // Exclude current element from both left and right sums.
        long long rightSum = totalSum - leftSum - nums[i];

        if (leftSum == rightSum) {
            return i;
        }

        // Current element becomes part of the left side
        // for the next index.
        leftSum += nums[i];
    }

    return -1;
}
```

### Dry run

`[1,7,3,6,5,6]`, total sum = 28.

At index 3, value = 6:

- Left sum = 1 + 7 + 3 = 11.
- Right sum = 28 - 11 - 6 = 11.
- Return index 3.

- **Time:** `O(n)`.
- **Auxiliary space:** `O(1)`.

### Interview follow-ups

1. **Does it work with negative values?** Yes; the total-sum identity still holds.
2. **Can there be multiple pivot indices?** Yes; return the first if that is the problem requirement, or collect all if asked.
3. **Why use `long long`?** To reduce overflow risk when summing many integers.

---

# 12. Move Zeroes

**Question:** Move all zeroes to the end while maintaining the relative order of nonzero elements. Modify the array in place.

**Example:** `[0,1,0,3,12]` → `[1,3,12,0,0]`.

## Brute force idea

Build a second array: append all nonzero elements in order, then append enough zeroes. Copy it back.

```cpp
#include <bits/stdc++.h>
using namespace std;

void moveZeroesBrute(vector<int>& nums) {
    vector<int> temp;

    // First collect nonzero elements in their original order.
    for (int x : nums) {
        if (x != 0) {
            temp.push_back(x);
        }
    }

    // Fill remaining positions with zeroes.
    while (temp.size() < nums.size()) {
        temp.push_back(0);
    }

    nums = temp;
}
```

- **Time:** `O(n)`.
- **Auxiliary space:** `O(n)`.

## Optimal: two pointers / write index

`write` is the next position where a nonzero value belongs. Scan once and write each nonzero value in order, then fill the remaining positions with zeroes.

```cpp
#include <bits/stdc++.h>
using namespace std;

void moveZeroes(vector<int>& nums) {
    int write = 0;

    // Compact all nonzero values at the beginning, preserving order.
    for (int read = 0; read < (int)nums.size(); read++) {
        if (nums[read] != 0) {
            nums[write] = nums[read];
            write++;
        }
    }

    // Every position from write onward must be zero.
    while (write < (int)nums.size()) {
        nums[write] = 0;
        write++;
    }
}
```

### Dry run

Input: `[0,1,0,3,12]`

- Read 0: skip.
- Read 1: write at index 0 → `[1,1,0,3,12]`.
- Read 0: skip.
- Read 3: write at index 1 → `[1,3,0,3,12]`.
- Read 12: write at index 2 → `[1,3,12,3,12]`.
- Fill indices 3 and 4 with zeroes → `[1,3,12,0,0]`.

- **Time:** `O(n)`.
- **Auxiliary space:** `O(1)`.

### Interview follow-ups

1. **Does it preserve order?** Yes; nonzero elements are written in the same order they are read.
2. **Can it be done with swaps?** Yes, use a pointer to the next zero slot and swap when a nonzero is found.
3. **Why is this in-place?** It only uses a constant number of indices and modifies the original vector.

---

# 13. Frequency of Array Elements

**Question:** Count how many times each value occurs in an array.

**Example:** `[1,2,2,3,1,1]` → `1:3, 2:2, 3:1`.

## Brute force

For each element, count its occurrences by scanning the entire array. Avoid recounting values already processed.

```cpp
#include <bits/stdc++.h>
using namespace std;

void frequencyBrute(vector<int>& nums) {
    int n = nums.size();

    for (int i = 0; i < n; i++) {
        // Skip if this value appeared earlier.
        bool alreadyCounted = false;
        for (int j = 0; j < i; j++) {
            if (nums[j] == nums[i]) {
                alreadyCounted = true;
                break;
            }
        }

        if (alreadyCounted) {
            continue;
        }

        int count = 0;

        // Count this value across the full array.
        for (int x : nums) {
            if (x == nums[i]) {
                count++;
            }
        }

        cout << nums[i] << " occurs " << count << " times\n";
    }
}
```

- **Time:** `O(n^2)`.
- **Auxiliary space:** `O(1)`.

## Optimal: unordered map

```cpp
#include <bits/stdc++.h>
using namespace std;

void frequencyCount(vector<int>& nums) {
    unordered_map<int, int> freq;

    // Each occurrence increments the count for its value.
    for (int x : nums) {
        freq[x]++;
    }

    // Print each distinct value and its total frequency.
    for (const auto& entry : freq) {
        cout << entry.first << " occurs "
             << entry.second << " times\n";
    }
}
```

### Dry run

Input: `[1,2,2,3,1,1]`

| Element processed | Frequency map |
|---:|---|
| 1 | `{1:1}` |
| 2 | `{1:1, 2:1}` |
| 2 | `{1:1, 2:2}` |
| 3 | `{1:1, 2:2, 3:1}` |
| 1 | `{1:2, 2:2, 3:1}` |
| 1 | `{1:3, 2:2, 3:1}` |

- **Expected time:** `O(n)`.
- **Space:** `O(m)` for `m` distinct values.

### Interview follow-ups

1. **When can a frequency array replace the map?** When the value range is small and known, such as values from 0 to 100.
2. **Why can map output order vary?** `unordered_map` does not maintain sorted order. Use `map` or sort the keys if ordered output is required.
3. **How do you find the most frequent value?** Track the maximum frequency while iterating over the map.

---

# Pattern recognition cheat sheet

| If the question asks… | Think of… |
|---|---|
| Has any value appeared before? | `unordered_set` |
| Do two strings have the same character counts? | Frequency array / map |
| Find a pair that adds to a target | Complement + hash map |
| Group items with the same structure | Canonical key + hash map |
| Find top K by frequency | Frequency map + heap / buckets |
| Encode arbitrary strings unambiguously | Length prefix |
| Product except current index | Prefix + suffix |
| Validate rows, columns and regions | Boolean tracking / sets |
| Longest consecutive run in unsorted data | Hash set; start only at sequence beginnings |
| Count subarrays with a target sum | Prefix sum + frequency map |
| Left sum equals right sum | Total sum + running prefix |
| Move zeroes / stable compaction | Read pointer + write pointer |
| Count occurrences | Frequency map / frequency array |

# C++ STL quick revision

```cpp
// Hash map: key -> value
unordered_map<int, int> mp;
mp[x]++;                         // Increment frequency
mp[x] = i;                       // Store / update value
mp.count(x);                     // 1 if key exists, otherwise 0
mp.find(x) != mp.end();          // Explicit existence check
mp.erase(x);                     // Remove key

// Hash set: unique values
unordered_set<int> st;
st.insert(x);
st.count(x);
st.find(x) != st.end();
st.erase(x);

// Ordered alternatives
map<int, int> orderedMap;        // Keys kept sorted; O(log n) operations
set<int> orderedSet;             // Unique sorted values; O(log n) operations

// Frequency array for lowercase English letters
int freq[26] = {};
freq[ch - 'a']++;
```

## Complexity table to memorise

| Data structure / operation | Average time | Worst-case / guaranteed note |
|---|---:|---|
| `unordered_map` find / insert | `O(1)` | Can degrade to `O(n)` |
| `unordered_set` find / insert | `O(1)` | Can degrade to `O(n)` |
| `map` find / insert | `O(log n)` | `O(log n)` |
| `set` find / insert | `O(log n)` | `O(log n)` |
| Sort `n` elements | `O(n log n)` | `O(n log n)` comparisons |
| Frequency array update | `O(1)` | `O(1)` |
| Heap push / pop | `O(log n)` | `O(log n)` |

## Interview explanation template

For every coding question, explain in this order:

1. **Observation:** What property of the input can we use?
2. **Brute force:** What is the simplest correct approach?
3. **Bottleneck:** Which repeated work makes it slow?
4. **Optimisation:** Which data structure or invariant removes that work?
5. **Dry run:** Trace a normal example and at least one edge case.
6. **Complexity:** State time and auxiliary space separately.
7. **Trade-off:** Mention when another approach might be preferable.

## Final self-check

Before moving to the next pattern, make sure you can do these without reading the solution:

- [ ] Explain when to use a set versus a map.
- [ ] Explain why Two Sum checks the complement before inserting the current value.
- [ ] Derive the frequency signature for anagrams.
- [ ] Explain bucket sort for Top K Frequent Elements.
- [ ] Explain prefix and suffix products without division.
- [ ] Derive the box index in Sudoku.
- [ ] Explain why Longest Consecutive Sequence only expands from sequence starts.
- [ ] Derive `prefix - k` for Subarray Sum Equals K.
- [ ] Derive `rightSum = totalSum - leftSum - current`.
- [ ] Explain the read/write pointer invariant for Move Zeroes.

**Preparation rule:** The company-specific sheet remains the source of truth. Use this document to learn the patterns and practise explaining the relevant questions; do not spend time solving every optional problem repeatedly if the Capgemini sheet does not prioritise it.
