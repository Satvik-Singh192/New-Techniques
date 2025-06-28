# Level-Order BFS - Longest Subsequence Repeated k Times

## Problem Link
[LeetCode 2014. Longest Subsequence Repeated k Times](https://leetcode.com/problems/longest-subsequence-repeated-k-times/)

## Technique Name
**Lexicographical BFS with Queue-Based Exploration**

## Key Intuition
Instead of depth-first exploration, we:
- Systematically explore all subsequences level by level (length 1, then 2, etc.)
- Process candidates in lexicographical order at each level
- Terminate early when no better solutions can be found

---

## 📝 Approach & Solution Journey

### Core Insights
✔ **Level-wise Processing:** Guarantees we find longest possible valid subsequence  
✔ **Lexicographical Sorting:** Maintains order within each level  
✔ **Early Termination:** Stops when no new candidates are generated  

### Solution Code
```cpp
class Solution {
public:
    string longestSubsequenceRepeatedK(string s, int k) {
        string result = "";
        queue<string> q;
        q.push("");
        while (!q.empty()) {
            string curr = q.front(); q.pop();
            for (char ch = 'a'; ch <= 'z'; ++ch) {
                string next = curr + ch;
                if (isK(next, s, k)) {
                    result = next;
                    q.push(next);
                }
            }
        }
        return result;
    }

    bool isK(string sub, string t, int k) {
        int count = 0, i = 0;
        for (char ch : t) {
            if (ch == sub[i]) {
                if (++i == sub.size()) {
                    i = 0;
                    if (++count == k) return true;
                }
            }
        }
        return false;
    }
};
