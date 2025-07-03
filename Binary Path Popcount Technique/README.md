# Binary Path Popcount Technique – Counting Increments in Doubling Split Strings

## Problem Link
[LeetCode 3304. Find the K-th Character in String Game I](https://leetcode.com/problems/find-the-k-th-character-in-string-game-i/)

---

## Technique Name
**Binary Path Popcount Technique (Bit Counting for Doubling Splits)**

---

## 📝 Initial Approach & Learning Journey

### Initial Attempt (Simulation)
- **Idea:** Start with `"a"` and repeatedly build the string by appending next-letter versions.
- **Implementation:** For each character, append its next character (with `'z' → 'a'` wraparound).
- **Stopping Condition:** Stop when the string length ≥ k.

✅ Worked for small k  
❌ Too slow for large k (e.g. k > 10^5)  

---

### What Went Wrong?
- **Brute Force Growth:** The string doubles in size each round → O(k) time and space.
- **Unnecessary Work:** Actually constructing the entire string when only 1 character is needed.
- **Missed Underlying Pattern:** Didn’t see that the recursion tree structure could be *traced* without building the string.

---

## 🎯 New Technique Learned: Binary Path Popcount Technique

### Core Insights
✔ At each step, the string is `original + incremented(original)`.  
✔ To find position `k`, think of repeated halving:
- If in **first half** → same letter.  
- If in **second half** → letter +1.  

✔ **Recursive path = binary representation of (k - 1):**
- Each bit tells you LEFT or RIGHT in recursion.
  - `0` bit → first half (no increment).  
  - `1` bit → second half (add +1).  

✔ **Number of 1-bits in (k - 1) = total +1 increments.**  
✔ Final character = `'a' + count_of_1_bits_in(k - 1)`.

---

### Example Trace
- **k = 5**  
  - k - 1 = 4 → binary `100`  
  - One `1` bit → one increment → `b`.

- **k = 11**  
  - k - 1 = 10 → binary `1010`  
  - Two `1` bits → two increments → `c`.

---

### Why It Works
✅ Each "second half" move in the recursive split = +1.  
✅ Each such move corresponds to a `1` bit in the path.  
✅ Counting 1s in (k - 1) directly computes total increments.  
✅ No need to simulate or store the entire string → pure arithmetic.

---

## 🧭 When to Use This Technique (Patterns to Look For)
- Problems where the structure **doubles each step** with predictable changes.  
- **Recursive splits** where one half is an unmodified copy and the other half is **transformed** (shifted/incremented/inverted).  
- Questions that describe **dividing the problem into halves repeatedly** (think recursion tree).  
- When the problem asks for a **specific position** in a *generated* string or sequence that would be too big to build.  
- Look for **left/right decisions** that can be encoded as bits:
  - Going left → no change  
  - Going right → apply transformation (+1, flip, etc.)  
- Analogous to **K-th Symbol in Grammar** or similar divide-and-conquer indexing problems.  

✅ If you can trace the position by halving, you can often **map the path to bits**.  
✅ Counting bits = counting transformations.

---

### Optimizations Discovered
1. Use **bit-counting builtin** for O(1) time:
   - `__builtin_popcount(k - 1)` (C++).  
2. No recursion or loop needed.  
3. Constant space.  

---

## 📊 Time & Space Complexity
- **Time:** `O(1)` (bit count on integer).  
- **Space:** `O(1)`.

---

## 💻 Final C++ Solution
```cpp
class Solution {
public:
    char kthCharacter(int k) {
        return 'a' + __builtin_popcount(k - 1);
    }
};
```
### 💡 Key Takeaways for Future Problems
- 🔹 **Recognize doubling patterns** – often represent binary-decision trees in disguise.
- 🔹 **Map recursion to binary paths** – LEFT/RIGHT choices correspond to bits.
- 🔹 **Count bits for transformations** – number of `1`s in index = number of applied operations.
- 🔹 **Avoid unnecessary simulation** – focus on how the result depends on choices, not on fully generating the structure.
- 🔹 This **bit-counting trick** can drastically simplify problems that seem exponential.
