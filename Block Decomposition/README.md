# 🧱 Block Decomposition (a.k.a. Square Root Decomposition)

Welcome to my Block Decomposition notes! This is my first time using this technique, and this `README.md` is here to help me revise it later or help others understand the basics.

## 📌 What is Block Decomposition?

**Block Decomposition** is a technique used to break a big problem into smaller chunks (blocks) to optimize certain operations like range queries or updates. It's not as powerful as a segment tree, but it's:

- Easier to implement
- Good for problems where you don’t need frequent updates
- A great stepping stone before diving into Segment Trees or Binary Indexed Trees

> ⚠️ It’s also called **Sqrt Decomposition** because block size is often chosen as `√n`.

---

## 📚 Key Idea

Instead of looping through the entire array every time (which can be too slow), you:

1. **Divide** the array into roughly `√n` blocks.
2. **Precompute** some useful info for each block (like max, sum, etc).
3. For each query, **skip full blocks** using the precomputed values whenever possible.
4. **Only scan individual elements** inside blocks when absolutely necessary.

---

## 🔍 When to Use?

Block decomposition is a solid choice when:

- The problem size is big (n up to 10⁵)
- You don’t need lightning-fast updates (segment tree is better for that)
- You're okay with `O(√n)` or `O(n√n)` performance
- You want something simpler than segment trees but faster than brute force

---

## 🛠️ Basic Setup (Revision Style)

```cpp
int n = arr.size();
int block_size = sqrt(n);
int block_count = (n + block_size - 1) / block_size;
vector<int> block_max(block_count, 0);

// Step 1: Precompute something (like max) for each block
for (int i = 0; i < n; i++) {
    int b = i / block_size;
    block_max[b] = max(block_max[b], arr[i]);
}

// Step 2: For each query, use block_max to skip entire blocks
// and only scan inside a block when needed
```
## ✅ Why It Works in My Problem

In **Leetcode 3479. Fruits Into Baskets III**, scanning the entire `baskets[]` array every time caused **TLE**.  
**Block Decomposition** helps by:

- 🧠 Precomputing the **max** of each block
- ⏭️ Skipping **entire blocks** when the max is less than `fruits[i]`
- 🔍 Scanning only the **minimal necessary** elements inside a block

This reduced the time complexity to around **O(n × √n)**, which was fast enough to pass all test cases ✅

---

## ⚠️ Limitations

- ❌ Not ideal for problems with **frequent or complex updates**
- 🚀 Segment Trees are **faster and more flexible**, but harder to implement
- 🪜 Block Decomposition is more like a **"quick win" solution** — simple to code, good enough in many cases, but not the best for everything
