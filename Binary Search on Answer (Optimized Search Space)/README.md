# Binary Search on Answer (Optimized Search Space) - Koko Eating Bananas

## Problem Link
[LeetCode 875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Technique Name
**Binary Search on Answer (or Binary Search on Solution Space)**

## Key Intuition
Instead of brute-forcing all possible values of `k`, we recognize that:
- The possible values of `k` lie in a **sorted range** (`[1, max(piles)]`).
- For a given `k`, we can check if it's feasible (`total_hours <= h`).
- Binary search helps us efficiently narrow down the minimal valid `k`.

---

## 📝 Initial Approach & Learning Journey

### Initial Attempt (Flawed Logic)
1. **Sorted the `piles` array** → Unnecessary, since eating order doesn’t matter.
2. **Tried to distribute `extra = h - n` hours** → Overcomplicated the problem by manually checking rightmost piles.
3. **Struggled to implement logic** → Realized the approach was not systematic.

### What Went Wrong?
- **Misapplied Greedy Thinking:** Assumed that manually adjusting `k` for the largest piles would work, but missed the global optimization needed.
- **Didn’t Recognize Binary Search Applicability:** Initially overlooked that `k` lies in a searchable range.

---

## 🎯 New Technique Learned: Binary Search on Answer

### Core Insights
✔ **Search Space:** `k` can range from `1` (slowest) to `max(piles)` (fastest).  
✔ **Feasibility Check:** For any `k`, compute total hours needed and compare with `h`.  
✔ **Binary Search:**  
   - If `hours <= h`, try a smaller `k` (search left half).  
   - If `hours > h`, try a larger `k` (search right half).  

### Optimizations Discovered
1. **Avoid Floating-Point Operations:**  
   - Use `(pile + k - 1) / k` instead of `ceil((double)pile / k)`.  
2. **Early Termination:**  
   - Break early in `canEatAll` if `sum > h`.  
3. **Cleaner Initialization:**  
   - `high = *max_element(piles.begin(), piles.end())` instead of manual loop.  

---

## 📊 Time & Space Complexity
- **Time:** `O(n log(max(piles)))`  
   - Binary search takes `O(log(max(piles)))`.  
   - Each feasibility check is `O(n)`.  
- **Space:** `O(1)` (no extra space used).  

---

## 💡 Key Takeaways for Future Problems
🔹 **When to Use Binary Search on Answer?**  
   - Problem requires minimizing/maximizing a value within a range.  
   - Feasibility can be checked for a given candidate.  
🔹 **Alternative Problems Using This Technique:**  
   - [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)  
   - [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)  

---
