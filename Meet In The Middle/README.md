# Meet-in-the-Middle Algorithm

## 🌟 Overview
Meet-in-the-middle (MitM) is a divide-and-conquer algorithm design pattern used to tackle problems involving large search spaces, especially where a naive brute-force solution is infeasible due to exponential complexity.

The idea is to split the input set into two halves, compute all possible outcomes (like subset sums) for each half, and then combine these results in an optimized way (e.g., binary search or hashing).

---

## ✅ When to Use This Approach
Look for these **patterns or signals** in a problem:

- The problem involves **subsets, partitions, or combinations** of elements.
- Input size `n` is **small to moderate** (typically up to 30 or 40).
- A brute-force solution would be `O(2^n)` — too large to compute directly.
- You want to **optimize a quantity (e.g., difference of sums, total cost, etc.)** using combinations or subsets.
- You can divide the problem into two independent halves and combine their results.

---

## 💡 What It Optimizes
- **Time Complexity**: Reduces `O(2^n)` to `O(2^{n/2} * log(2^{n/2}))` (roughly `O(n * 2^{n/2})`).
- **Search Complexity**: Makes it feasible to find optimal solutions in an exponential search space by reducing the size of the space.

---

## ⚖️ Time Complexity

| Phase                         | Complexity                  |
|------------------------------|-----------------------------|
| Before Meet-in-the-Middle    | `O(2^n)`                    |
| After Meet-in-the-Middle     | `O(2^{n/2} * log(2^{n/2}))` |

---

## 🔑 Key Components

- **Subset generation** on both halves independently.
- **Storing subset sums** (or other computed values) in lists.
- **Sorting** one list to enable binary search.
- **Combining** results from both halves to get the final answer (e.g., minimize difference).

---

## 💪 Example Problem: Minimum Subset Sum Difference

**Problem**: Given an array of `2n` integers, split it into two groups of `n` elements each such that the absolute difference between their sums is minimized.

### Code Skeleton:
```cpp
class Solution {
public:
    void fill_subset_sum(const vector<int>& nums, int idx, int end, long long sum, int count, vector<vector<long long>>& out) {
        if (idx > end) {
            out[count].push_back(sum);
            return;
        }
        fill_subset_sum(nums, idx + 1, end, sum, count, out);
        fill_subset_sum(nums, idx + 1, end, sum + nums[idx], count + 1, out);
    }

    int minimumDifference(vector<int>& nums) {
        int N = nums.size();       
        int n = N / 2;             
        long long total = accumulate(nums.begin(), nums.end(), 0LL);

        vector<vector<long long>> leftSums(n + 1), rightSums(n + 1);

        fill_subset_sum(nums, 0, n - 1, 0, 0, leftSums);
        fill_subset_sum(nums, n, N - 1, 0, 0, rightSums);

        for (int k = 0; k <= n; ++k) {
            sort(leftSums[k].begin(), leftSums[k].end());
            sort(rightSums[k].begin(), rightSums[k].end());
        }

        int ans = INT_MAX;
        long long half = total / 2;

        for (int k = 0; k <= n; ++k) {
            auto& L = leftSums[k];
            auto& R = rightSums[n - k];

            for (long long sumL : L) {
                long long need = half - sumL;
                auto it = lower_bound(R.begin(), R.end(), need);

                if (it != R.end()) {
                    ans = min(ans, abs(int(total - 2 * (sumL + *it))));
                }
                if (it != R.begin()) {
                    --it;
                    ans = min(ans, abs(int(total - 2 * (sumL + *it))));
                }
            }
        }

        return ans;
    }
};
```
## 📄 Other Problems Using Meet-in-the-Middle

- [Leetcode 1755 - Closest Subsequence Sum](https://leetcode.com/problems/closest-subsequence-sum/)
- [Leetcode 2035 - Partition Array Into Two Arrays to Minimize Sum Difference](https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/)
- [AtCoder Educational DP Contest - G: Longest Path](https://atcoder.jp/contests/dp/tasks/dp_g) *(modified approach)*
- Subset sum variants like bounded/unbounded knapsack with small sizes.

---

## 🔹 Summary

Meet-in-the-middle is a **powerful technique** when brute-force is too slow but the input is still small enough to split in two.

It's all about **reducing 2^n brute-force to 2^{n/2}**, making previously intractable problems solvable.

It usually comes into play in **subset sum, partitioning, or search optimization problems**.

Use it wisely when you see:

- Small `n` (around 30 or less).
- Need to explore all subsets or combinations.
- Can divide the input into two roughly equal parts.
- You can match/combine results using **binary search or hash map**.

---

## ✍️ Handy Tips

- You can optimize space using maps or keeping only needed subset sizes.
- Sorting one half and binary searching helps **quickly find complements**.
- Use `long long` when handling subset sums to avoid overflow.
- Precompute and cache values if subset sums are reused.
