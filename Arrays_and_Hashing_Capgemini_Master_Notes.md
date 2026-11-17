# Arrays + Hashing — Capgemini DSA Interview Preparation

> **Language:** C++  
> **Level:** Beginner to Interview-Ready  
> **Focus:** Pattern identification, brute force, optimized solutions, dry runs, complexity and interview follow-ups.

---

## Table of Contents

1. HashMap and HashSet Fundamentals
2. Two Sum — LeetCode 1
3. Contains Duplicate — LeetCode 217
4. Majority Element — LeetCode 169
   - HashMap approach
   - Boyer–Moore Voting Algorithm
5. Valid Anagram — LeetCode 242
6. First Unique Character — LeetCode 387
7. Group Anagrams — LeetCode 49
8. Top K Frequent Elements — LeetCode 347
9. Single Number — LeetCode 136
10. Missing Number — LeetCode 268
11. Optional Interview Extensions
    - Intersection of Two Arrays II
    - Longest Consecutive Sequence
12. Pattern Identification Cheat Sheet
13. Complexity Revision
14. Interview Follow-up Questions

---

# 1. HashMap and HashSet Fundamentals

## What is Hashing?

Hashing is a technique that allows us to store and retrieve information efficiently using a key.

Instead of repeatedly traversing an array to search for an element, we store information in a hash-based data structure and retrieve it quickly.

### unordered_map

Stores key-value pairs.

```cpp
unordered_map<int, int> mp;

mp[5]++;                  // Insert or increase frequency
mp[5] = 10;               // Update value
mp.count(5);              // Check whether key exists
mp.find(5) != mp.end();   // Another existence check
mp.erase(5);              // Delete key
```

Example:

```cpp
vector<int> arr = {2, 3, 2, 5, 3, 2};

unordered_map<int, int> freq;

for (int x : arr) {
    freq[x]++;
}
```

Frequency map:

| Key | Frequency |
|---|---:|
| 2 | 3 |
| 3 | 2 |
| 5 | 1 |

### unordered_set

Stores only unique values.

```cpp
unordered_set<int> st;

st.insert(10);
st.insert(20);

if (st.count(10)) {
    cout << "Present";
}
```

Use a set when you only need to know whether an element exists.

Use a map when you need to associate a value with another value, such as frequency or index.

### Complexity

| Operation | Average | Worst Case |
|---|---:|---:|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |

Hash-based operations are average O(1), not guaranteed O(1). Worst-case performance can degrade because of hash collisions.

## When to Identify Hashing?

Look for these clues:

- Find whether an element exists.
- Count the frequency of elements.
- Find duplicates.
- Find a pair with a target sum.
- Store previously visited values.
- Group elements based on a common property.
- Count subarrays with a particular sum.
- Find the first unique or most frequent element.

**Core intuition:** Instead of repeatedly searching the array, remember what you have already seen.

---

# 2. Two Sum — LeetCode 1

**Link:** https://leetcode.com/problems/two-sum/

**Pattern:** Array + HashMap

## Problem

Given an array of integers and a target, return the indices of two numbers whose sum equals the target.

Example:

```cpp
nums = {2, 7, 11, 15};
target = 9;

// Output: {0, 1}
```

Because:

```text
nums[0] + nums[1] = 2 + 7 = 9
```

## Approach 1: Brute Force

Check every possible pair.

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {

        int n = nums.size();

        for (int i = 0; i < n; i++) {

            for (int j = i + 1; j < n; j++) {

                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }

        return {};
    }
};
```

**Complexity:**

- Time: O(n²)
- Space: O(1)

## Approach 2: Optimal — HashMap

### Intuition

Suppose:

```text
a + b = target
```

Rearranging:

```text
b = target - a
```

For every current element, calculate its required complement and check whether that complement has already appeared.

We store:

```text
Number -> Index
```

### Dry Run

```text
nums = [2, 7, 11, 15]
target = 9
```

| Iteration | Current | Required Complement | Map | Action |
|---|---:|---:|---|---|
| 1 | 2 | 7 | {} | Store 2 → 0 |
| 2 | 7 | 2 | {2:0} | Found 2! Return {0,1} |

### C++ Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {

        // Number -> Index
        unordered_map<int, int> mp;

        for (int i = 0; i < nums.size(); i++) {

            // Calculate required number
            int complement = target - nums[i];

            // Check whether complement was seen earlier
            if (mp.find(complement) != mp.end()) {
                return {mp[complement], i};
            }

            // Store current number and its index
            mp[nums[i]] = i;
        }

        return {};
    }
};
```

**Complexity:**

- Time: O(n) average
- Space: O(n)

### Interview Follow-ups

**Q1. Why do we check before inserting?**

To ensure that we do not use the same element twice.

**Q2. What if duplicate elements exist?**

For `[3,3]`, target `6`, the first 3 is stored and the second 3 finds it.

**Q3. Can we solve it using sorting and two pointers?**

Yes, but sorting changes the original order. To return original indices, we must preserve the indices alongside the values.

---

# 3. Contains Duplicate — LeetCode 217

**Link:** https://leetcode.com/problems/contains-duplicate/

**Pattern:** Array + HashSet

## Problem

Return true if any value appears at least twice.

Example:

```cpp
nums = {1, 2, 3, 1};

// Output: true
```

## Approach 1: Brute Force

Compare every pair.

- Time: O(n²)
- Space: O(1)

## Approach 2: Optimal — HashSet

### Intuition

We only need to know whether a number has appeared before.

Therefore, `unordered_set` is sufficient.

### Dry Run

Input:

```text
[1, 2, 3, 1]
```

| Current | Already Present? | Set |
|---|---|---|
| 1 | No | {1} |
| 2 | No | {1,2} |
| 3 | No | {1,2,3} |
| 1 | Yes | Return true |

### C++ Code

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {

        // Store visited numbers
        unordered_set<int> seen;

        for (int num : nums) {

            // Duplicate found
            if (seen.count(num)) {
                return true;
            }

            // Store current number
            seen.insert(num);
        }

        return false;
    }
};
```

**Complexity:**

- Time: O(n) average
- Space: O(n)

### Interview Follow-ups

1. What if the array is sorted? Compare adjacent elements in O(n) time and O(1) extra space.
2. What if we need to return the duplicate number? Return `num` when it is already present.
3. Why use a set instead of a map? We only need existence, not an associated value.

---

# 4. Majority Element — LeetCode 169

**Link:** https://leetcode.com/problems/majority-element/

**Pattern:** HashMap / Boyer–Moore Voting Algorithm

## Problem

Find the element that appears more than n/2 times.

Example:

```cpp
nums = {2, 2, 1, 1, 1, 2, 2};

// Output: 2
```

Array size = 7.

Majority element must occur more than 3.5 times.

Element 2 occurs 4 times.

## Approach 1: HashMap Frequency

### Intuition

Count the frequency of every number and return the one whose frequency exceeds n/2.

### C++ Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {

        unordered_map<int, int> freq;

        for (int num : nums) {

            freq[num]++;

            // Check whether frequency exceeds n/2
            if (freq[num] > nums.size() / 2) {
                return num;
            }
        }

        return -1;
    }
};
```

**Complexity:**

- Time: O(n) average
- Space: O(n)

## Approach 2: Boyer–Moore Voting Algorithm

**Important interview optimization**

### Core Intuition

Imagine every occurrence of a majority element as one vote.

When a different element appears, it cancels one vote.

Because the majority element occurs more than all other elements combined, it cannot be completely eliminated.

### Two Variables

```cpp
int candidate = 0;
int count = 0;
```

- `candidate`: Current majority candidate.
- `count`: Number of active votes.

### Three Rules

1. If `count == 0`, choose the current element as the candidate.
2. If current element equals candidate, increment count.
3. Otherwise, decrement count.

### Dry Run

Input:

```cpp
nums = {2, 2, 1, 1, 1, 2, 2};
```

| Number | Candidate | Count | Action |
|---:|---:|---:|---|
| 2 | 2 | 1 | New candidate |
| 2 | 2 | 2 | Same → increment |
| 1 | 2 | 1 | Different → decrement |
| 1 | 2 | 0 | Different → decrement |
| 1 | 1 | 1 | New candidate |
| 2 | 1 | 0 | Different → decrement |
| 2 | 2 | 1 | New candidate |

Final candidate = 2.

### C++ Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {

        int candidate = 0;
        int count = 0;

        for (int num : nums) {

            // No active candidate
            if (count == 0) {
                candidate = num;
            }

            // Same element supports candidate
            if (num == candidate) {
                count++;
            }

            // Different element cancels one vote
            else {
                count--;
            }
        }

        return candidate;
    }
};
```

**Complexity:**

- Time: O(n)
- Space: O(1)

### Important: What if majority is not guaranteed?

Boyer–Moore finds a candidate, but that candidate is not necessarily a majority element if the problem does not guarantee one exists.

Verify its frequency:

```cpp
int freq = 0;

for (int num : nums) {
    if (num == candidate) {
        freq++;
    }
}

if (freq > nums.size() / 2) {
    return candidate;
}

return -1;
```

### Interview Follow-ups

**Q1. Why does Boyer–Moore work?**

Because each majority occurrence can be paired with a non-majority occurrence and cancelled. Since majority occurrences are more numerous, some must survive.

**Q2. Can we solve it in O(1) extra space?**

Yes, Boyer–Moore.

**Q3. What if we need elements occurring more than n/3 times?**

Use the extended Boyer–Moore algorithm with two candidates. At most two elements can satisfy this condition.

---

# 5. Valid Anagram — LeetCode 242

**Link:** https://leetcode.com/problems/valid-anagram/

**Pattern:** Frequency Counting

## Problem

Determine whether two strings contain exactly the same characters with the same frequencies.

Example:

```cpp
s = "anagram";
t = "nagaram";

// Output: true
```

## Approach 1: Sorting

Sort both strings and compare them.

- Time: O(n log n)
- Space: Depends on sorting implementation.

## Approach 2: Frequency Array

For lowercase English letters, use an array of size 26.

### C++ Code

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {

        // Different lengths cannot be anagrams
        if (s.size() != t.size()) {
            return false;
        }

        int freq[26] = {0};

        for (int i = 0; i < s.size(); i++) {

            // Increase frequency for s
            freq[s[i] - 'a']++;

            // Decrease frequency for t
            freq[t[i] - 'a']--;
        }

        // Every frequency must be zero
        for (int count : freq) {
            if (count != 0) {
                return false;
            }
        }

        return true;
    }
};
```

### Dry Run

```text
s = "anagram"
t = "nagaram"
```

Every character has the same frequency in both strings.

Therefore, all frequency differences become zero.

Output: `true`.

**Complexity:**

- Time: O(n)
- Space: O(1), because the frequency array always has 26 entries.

### Interview Follow-ups

1. What if arbitrary characters or Unicode are allowed? Use an appropriate map or character representation.
2. What if comparison is case-insensitive? Normalize case first.
3. What if spaces and punctuation should be ignored? Normalize both strings before frequency counting.

---

# 6. First Unique Character in a String — LeetCode 387

**Link:** https://leetcode.com/problems/first-unique-character-in-a-string/

**Pattern:** Frequency Counting

## Problem

Return the index of the first character that appears exactly once.

If no such character exists, return -1.

Example:

```cpp
s = "leetcode";

// Output: 0
```

## Approach: Two-Pass Frequency Counting

### Intuition

1. First pass: Count every character.
2. Second pass: Find the first character whose frequency is 1.

### Dry Run

Input: `"leetcode"`

| Character | Frequency |
|---|---:|
| l | 1 |
| e | 3 |
| t | 1 |
| c | 1 |
| o | 1 |
| d | 1 |

The first unique character is `l`, at index 0.

### C++ Code

```cpp
class Solution {
public:
    int firstUniqChar(string s) {

        int freq[26] = {0};

        // First pass: Count frequencies
        for (char ch : s) {
            freq[ch - 'a']++;
        }

        // Second pass: Find first unique character
        for (int i = 0; i < s.size(); i++) {

            if (freq[s[i] - 'a'] == 1) {
                return i;
            }
        }

        return -1;
    }
};
```

**Complexity:**

- Time: O(n)
- Space: O(1) for lowercase English letters.

### Interview Follow-ups

1. Why two passes? The first pass calculates complete frequencies; the second preserves original order.
2. Why not return when a character first appears? A later occurrence may make it repeated.
3. What if the character set is unrestricted? Use an unordered map.

---

# 7. Group Anagrams — LeetCode 49

**Link:** https://leetcode.com/problems/group-anagrams/

**Pattern:** HashMap + Sorting

## Problem

Group strings that are anagrams of one another.

Example:

```cpp
strs = {"eat", "tea", "tan", "ate", "nat", "bat"};
```

Possible output:

```text
[
  ["eat", "tea", "ate"],
  ["tan", "nat"],
  ["bat"]
]
```

## Approach: HashMap + Sorted String Key

### Intuition

Anagrams have the same sorted representation.

```text
eat → aet
tea → aet
ate → aet
```

Therefore, use the sorted string as a common key.

```text
Sorted String → Original Strings
```

### Dry Run

| Original | Sorted Key | Group |
|---|---|---|
| eat | aet | [eat] |
| tea | aet | [eat, tea] |
| tan | ant | [tan] |
| ate | aet | [eat, tea, ate] |
| nat | ant | [tan, nat] |
| bat | abt | [bat] |

### C++ Code

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {

        // Sorted string -> Original strings
        unordered_map<string, vector<string>> mp;

        for (string word : strs) {

            // Generate common key
            string key = word;

            sort(key.begin(), key.end());

            // Add original word to corresponding group
            mp[key].push_back(word);
        }

        vector<vector<string>> ans;

        // Collect all groups
        for (auto &entry : mp) {
            ans.push_back(entry.second);
        }

        return ans;
    }
};
```

Let:

- n = number of strings.
- k = maximum length of a string.

**Complexity:**

- Time: O(n × k log k)
- Space: O(n × k), including stored keys and grouped strings.

### Interview Follow-ups

1. Can sorting be avoided? Yes. Use a 26-character frequency signature as the key.
2. Why use a HashMap? It groups strings with the same signature.
3. Does output order matter? Usually not unless specified.

---

# 8. Top K Frequent Elements — LeetCode 347

**Link:** https://leetcode.com/problems/top-k-frequent-elements/

**Pattern:** HashMap + Heap

This question combines two concepts:

1. HashMap for frequency counting.
2. Priority Queue for selecting the k most frequent elements.

## Problem

Return the k most frequent elements.

Example:

```cpp
nums = {1, 1, 1, 2, 2, 3};
k = 2;

// Output: {1, 2}
```

## Step 1: Frequency Counting

```text
1 → 3
2 → 2
3 → 1
```

## Step 2: Min-Heap of Size k

A min-heap keeps the smallest frequency at the top.

We maintain only k elements.

When heap size exceeds k, remove the element with the smallest frequency.

### Dry Run

For k = 2:

| Insert | Heap Contents | Action |
|---|---|---|
| {3,1} | {3,1} | Insert |
| {2,2} | {2,2}, {3,1} | Insert |
| {1,3} | {1,3}, {2,2}, {3,1} | Remove lowest frequency |

Remaining elements: 1 and 2.

### C++ Code

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {

        // Step 1: Count frequencies
        unordered_map<int, int> freq;

        for (int num : nums) {
            freq[num]++;
        }

        // Step 2: Min-heap storing {frequency, number}
        priority_queue<
            pair<int, int>,
            vector<pair<int, int>>,
            greater<pair<int, int>>
        > minHeap;

        for (auto &entry : freq) {

            int num = entry.first;
            int count = entry.second;

            minHeap.push({count, num});

            // Maintain heap size k
            if (minHeap.size() > k) {
                minHeap.pop();
            }
        }

        // Step 3: Extract remaining elements
        vector<int> ans;

        while (!minHeap.empty()) {
            ans.push_back(minHeap.top().second);
            minHeap.pop();
        }

        return ans;
    }
};
```

Let:

- n = array length.
- m = number of distinct elements.

**Complexity:**

- Frequency counting: O(n) average.
- Heap operations: O(m log k).
- Total: O(n + m log k).
- Space: O(m + k).

### Interview Follow-ups

**Q1. Why use a min-heap?**

The least frequent among the selected k elements stays at the top, allowing us to remove it when a better candidate arrives.

**Q2. Why not sort all frequencies?**

Sorting takes O(m log m). A size-k heap is useful when k is relatively small.

**Q3. Can we solve it in O(n)?**

Yes. Bucket sort can group elements by frequency because the maximum frequency cannot exceed n.

**Q4. What is the default priority_queue in C++?**

A max-heap.

Use `greater<>` to create a min-heap.

---

# 9. Single Number — LeetCode 136

**Link:** https://leetcode.com/problems/single-number/

**Primary Pattern:** Bit Manipulation — XOR

This is an array problem, but its optimal solution is not HashMap.

## Problem

Every element appears twice except one. Find the unique element.

Example:

```cpp
nums = {4, 1, 2, 1, 2};

// Output: 4
```

## XOR Properties

```text
a ^ a = 0
a ^ 0 = a
a ^ b ^ a = b
```

Duplicate numbers cancel each other.

## Dry Run

```text
4 ^ 1 ^ 2 ^ 1 ^ 2

= 4 ^ (1 ^ 1) ^ (2 ^ 2)

= 4 ^ 0 ^ 0

= 4
```

## C++ Code

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {

        int ans = 0;

        // Duplicate numbers cancel out
        for (int num : nums) {
            ans ^= num;
        }

        return ans;
    }
};
```

**Complexity:**

- Time: O(n)
- Space: O(1)

### Interview Follow-ups

1. What if every number appears three times except one? Use bit counting with modulo 3.
2. Why is XOR better than HashMap? It uses constant extra space.
3. What if two unique numbers exist and all others appear twice? XOR all values and partition by a set bit.

---

# 10. Missing Number — LeetCode 268

**Link:** https://leetcode.com/problems/missing-number/

**Primary Pattern:** Mathematics / XOR

## Problem

An array contains n distinct numbers from the range [0,n]. Find the missing number.

Example:

```cpp
nums = {3, 0, 1};

// Output: 2
```

## Approach 1: Mathematics

Expected sum:

```text
n × (n + 1) / 2
```

Missing number:

```text
Expected sum - Actual sum
```

### Dry Run

```text
nums = [3, 0, 1]
n = 3

Expected sum = 3 × 4 / 2 = 6

Actual sum = 3 + 0 + 1 = 4

Missing number = 6 - 4 = 2
```

### C++ Code

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {

        int n = nums.size();

        // Expected sum from 0 to n
        long long expected = 1LL * n * (n + 1) / 2;

        long long actual = 0;

        // Calculate actual sum
        for (int num : nums) {
            actual += num;
        }

        return expected - actual;
    }
};
```

**Complexity:**

- Time: O(n)
- Space: O(1)

### Approach 2: XOR

XOR all numbers from 0 to n and XOR all array elements.

The duplicate values cancel, leaving the missing number.

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {

        int ans = nums.size();

        for (int i = 0; i < nums.size(); i++) {
            ans ^= i;
            ans ^= nums[i];
        }

        return ans;
    }
};
```

**Complexity:**

- Time: O(n)
- Space: O(1)

### Interview Follow-ups

1. Why use long long in the sum approach? To prevent integer overflow.
2. Can XOR solve it? Yes, because paired values cancel.
3. What if duplicates are allowed? The basic sum/XOR solutions no longer directly apply.

---

# 11. Optional Interview Extensions

These are useful additions but should not delay completion of the core pattern.

## A. Intersection of Two Arrays II — LeetCode 350

**Link:** https://leetcode.com/problems/intersection-of-two-arrays-ii/

**Pattern:** HashMap Frequency Counting

### Problem

Return the intersection of two arrays, including duplicate occurrences.

Example:

```cpp
nums1 = {1, 2, 2, 1};
nums2 = {2, 2};

// Output: {2, 2}
```

### C++ Code

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {

        unordered_map<int, int> freq;

        // Count occurrences in first array
        for (int num : nums1) {
            freq[num]++;
        }

        vector<int> ans;

        // Match occurrences from second array
        for (int num : nums2) {

            if (freq[num] > 0) {
                ans.push_back(num);
                freq[num]--;
            }
        }

        return ans;
    }
};
```

**Complexity:**

- Time: O(n + m) average.
- Space: O(n).

### Follow-up

What if both arrays are sorted? Use two pointers.

---

## B. Longest Consecutive Sequence — LeetCode 128

**Link:** https://leetcode.com/problems/longest-consecutive-sequence/

**Pattern:** HashSet

### Problem

Find the length of the longest consecutive sequence.

Example:

```cpp
nums = {100, 4, 200, 1, 3, 2};

// Output: 4
```

Longest sequence:

```text
1, 2, 3, 4
```

### Intuition

Store all values in a HashSet.

Only start counting from a number if its predecessor does not exist.

For example:

- For 2, check whether 1 exists.
- Since 1 exists, 2 is not the beginning of a sequence.
- For 1, predecessor 0 does not exist, so start counting.

### C++ Code

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {

        unordered_set<int> st;

        // Store unique values
        for (int num : nums) {
            st.insert(num);
        }

        int longest = 0;

        for (int num : st) {

            // Start only at the beginning of a sequence
            if (st.find(num - 1) == st.end()) {

                int current = num;
                int length = 1;

                // Count consecutive elements
                while (st.find(current + 1) != st.end()) {
                    current++;
                    length++;
                }

                longest = max(longest, length);
            }
        }

        return longest;
    }
};
```

**Complexity:**

- Time: O(n) average.
- Space: O(n).

### Interview Follow-ups

1. Why use a HashSet instead of sorting? It gives average O(1) lookup and avoids O(n log n) sorting.
2. Why check `num - 1`? To ensure each consecutive sequence is counted only once.
3. What if the array contains duplicates? The set automatically removes duplicates.

---

# 12. Pattern Identification Cheat Sheet

| Problem Clue | Think Of |
|---|---|
| Find a pair with a target sum | HashMap |
| Check whether an element exists | HashSet |
| Find duplicates | HashSet |
| Count frequencies | HashMap |
| Find majority element | Boyer–Moore / HashMap |
| Same character frequencies | Frequency Array |
| First unique character | Frequency Counting |
| Group anagrams | HashMap + Sorted Key |
| Top K frequent elements | HashMap + Heap |
| Every number appears twice except one | XOR |
| Find missing number in [0,n] | Mathematics / XOR |
| Longest consecutive sequence | HashSet |
| Count subarrays with sum K | Prefix Sum + HashMap |

---

# 13. Complexity Revision

| Question | Optimal Pattern | Time | Extra Space |
|---|---|---:|---:|
| Two Sum | HashMap | O(n) average | O(n) |
| Contains Duplicate | HashSet | O(n) average | O(n) |
| Majority Element | Boyer–Moore | O(n) | O(1) |
| Valid Anagram | Frequency Array | O(n) | O(1) |
| First Unique Character | Frequency Counting | O(n) | O(1) |
| Group Anagrams | HashMap + Sorting | O(n × k log k) | O(nk) |
| Top K Frequent | HashMap + Heap | O(n + m log k) | O(m + k) |
| Single Number | XOR | O(n) | O(1) |
| Missing Number | Mathematics / XOR | O(n) | O(1) |
| Intersection of Arrays II | HashMap | O(n + m) average | O(n) |
| Longest Consecutive Sequence | HashSet | O(n) average | O(n) |

---

# 14. Important Interview Follow-up Questions

### Hashing

1. Difference between `unordered_map` and `map`?
2. What are hash collisions?
3. What is the average and worst-case complexity of unordered_map?
4. Difference between `unordered_set` and `unordered_map`?
5. When would you use a frequency array instead of a HashMap?

### Arrays

6. How do you find duplicates in an array?
7. How do you find the majority element in O(n) time and O(1) space?
8. How do you find the missing number without sorting?
9. How do you find the first non-repeating element?
10. How do you group anagrams efficiently?

### Heap and Priority Queue

11. Difference between min-heap and max-heap?
12. What is the default priority queue in C++?
13. How do you maintain a heap of size K?
14. Why is a min-heap useful for Top K Frequent Elements?
15. What is the complexity of insertion and deletion in a heap?

### Bit Manipulation

16. What is XOR?
17. Why does `a ^ a = 0`?
18. How does XOR help solve Single Number?
19. How can XOR help find a missing number?

---

# 15. Important Pattern Boundaries

Do not mix these patterns simply because they all operate on arrays.

| Question | Correct Primary Pattern | Separate Section |
|---|---|---|
| Two Sum | HashMap | Arrays + Hashing |
| Contains Duplicate | HashSet | Arrays + Hashing |
| Majority Element | Boyer–Moore | Arrays + Hashing |
| Valid Anagram | Frequency Counting | Arrays + Hashing |
| Group Anagrams | HashMap | Arrays + Hashing |
| Top K Frequent | HashMap + Heap | Arrays + Hashing / Heap |
| Single Number | XOR | Bit Manipulation |
| Missing Number | Math / XOR | Mathematics / Bit Manipulation |
| Product of Array Except Self | Prefix/Suffix | Separate Prefix/Suffix Pattern |
| Subarray Sum Equals K | Prefix Sum + HashMap | Separate Prefix Sum Pattern |
| Longest Substring Without Repeating Characters | Sliding Window + HashMap | Separate Sliding Window Pattern |
| 3Sum | Sorting + Two Pointers | Separate Two Pointers Pattern |

**Product of Array Except Self — LeetCode 238**

https://leetcode.com/problems/product-of-array-except-self/

We will cover this in the dedicated Prefix/Suffix Arrays section, not here.

---

## Final Revision Strategy

Before moving to the next pattern:

- [ ] Explain HashMap vs HashSet.
- [ ] Implement Two Sum without looking at the code.
- [ ] Implement Contains Duplicate.
- [ ] Explain Boyer–Moore Voting Algorithm with a dry run.
- [ ] Implement Valid Anagram.
- [ ] Explain why First Unique Character needs two passes.
- [ ] Implement Group Anagrams.
- [ ] Explain min-heap in Top K Frequent Elements.
- [ ] Explain XOR properties for Single Number.
- [ ] Solve Missing Number using sum and XOR.
- [ ] State time and space complexity for every solution.

**Interview rule:** Never jump straight to code. Explain the brute-force approach, identify its limitation, derive the optimized approach, dry-run it, and then write the code.

**Arrays + Hashing: understand the pattern, not just memorize the solutions.**

---

*Prepared for Capgemini DSA Technical Interview Preparation — C++ Edition.*
