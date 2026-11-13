# Evaluate Reverse Polish Notation

| | |
|---|---|
| **Section** | 6. Stacks & Queues |
| **Priority** | P1 |
| **Difficulty** | Medium |
| **Key pattern** | Stack |
| **Problem** | [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) |

> Framework: Understand → Brute Force → Brute-Force Complexity → Bottleneck → Optimal → C++ Code → Dry Run → Edge Cases

---

## 1. Understand the Problem

Evaluate an expression in postfix notation, e.g. `["2","1","+","3","*"]` = `(2+1)*3 = 9`. Division truncates toward zero.

**Clarify:** Always a valid expression? Integer division only?

## 2. Brute Force

Convert to infix with parentheses and parse, which is unnecessarily complex.

## 3. Brute-Force Complexity

Hard to implement; typically `O(n)` but error-prone.

## 4. Bottleneck

Postfix is designed for stack evaluation: operators apply to the two most recent operands.

## 5. Optimal Approach (and why it works)

Scan tokens. Numbers → push. Operator → pop `b` (top) then `a`, compute `a op b` (order matters for `-` and `/`), push the result. The single remaining value is the answer. C++ integer division already truncates toward zero.

**Complexity:** Time `O(n)`, Space `O(n)`.

## 6. Clean C++ Code

*(assume `#include <bits/stdc++.h>` and `using namespace std;`)*

```cpp
int evalRPN(vector<string>& tokens) {
    stack<long long> st;
    for (const string& t : tokens) {
        if (t == "+" || t == "-" || t == "*" || t == "/") {
            long long b = st.top(); st.pop();          // second operand
            long long a = st.top(); st.pop();          // first operand
            if (t == "+") st.push(a + b);
            else if (t == "-") st.push(a - b);
            else if (t == "*") st.push(a * b);
            else st.push(a / b);                       // truncates toward zero
        } else {
            st.push(stoll(t));                         // handles negatives like "-11"
        }
    }
    return (int)st.top();
}
```

## 7. Dry Run

`["4","13","5","/","+"]`

- push `4`, `13`, `5`
- `/`: `b=5, a=13` → `13/5 = 2` → stack `[4, 2]`
- `+`: `b=2, a=4` → `6`
- Answer `6`

## 8. Edge Cases

- Single number `["7"]` → `7`
- Negative numbers: token `"-11"` is a number, not the `-` operator (compare the whole string)
- Division with negatives `-7/2 = -3` (truncation)
- Operand order for `-` and `/`
- Overflow → `long long` stack

## Likely Follow-ups

- Convert infix to postfix? → Shunting-yard algorithm.
- Why compare full strings? → `"-1"` starts with `-` but is an operand.
