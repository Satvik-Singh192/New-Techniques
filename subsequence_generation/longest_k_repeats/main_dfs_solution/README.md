# Reverse Lexicographical DFS with Pruning - Longest Subsequence Repeated k Times

## Problem Link
[LeetCode 2014. Longest Subsequence Repeated k Times](https://leetcode.com/problems/longest-subsequence-repeated-k-times/)

## Technique Name
**Reverse Lexicographical DFS with Early Pruning**

## Key Intuition
Instead of brute-forcing all possible subsequences, we recognize that:
- The solution must consist of characters appearing ≥k times
- The lexicographically largest solution can be found by trying characters from 'z' to 'a'
- We can prune invalid branches early to optimize the search

---

## 📝 Approach & Solution Journey

### Core Insights
✔ **Pre-filtering:** Remove characters with frequency < k  
✔ **Reverse Order Search:** Process characters from 'z' to 'a'  
✔ **Pruning Conditions:**  
   - Stop if current length exceeds `maxLen = n/k`  
   - Validate candidates with O(n) `repeatK` check  
   - Update answer only when finding longer valid subsequences  

### Solution Code
```cpp
class Solution {
public:
    int n, maxLen=0;
    string ans="";
    vector<char> chars;
    
    bool repeatK(string& s, string& t, int m, int k) {
        for (int i=0, j=0; i<n && k>0; i++) {
            if (s[i]==t[j] && ++j == m) {
                k--; j=0;
            }
        }
        return k==0;
    }

    void dfs(string& s, string& t, int k) {
        if (t.size() > maxLen) return;
        if (!t.empty() && !repeatK(s, t, t.size(), k)) return;
        if (t.size() > ans.size()) ans = t;
        
        for (char c : chars) {
            t.push_back(c);
            dfs(s, t, k);
            t.pop_back();
        }
    }

    string longestSubsequenceRepeatedK(string& s, int k) {
        int freq[26]={0};
        bitset<26> hasX=0;
        for(char c: s) if (++freq[c-'a']==k) hasX[c-'a']=1;
        if (hasX==0) return "";

        n = s.size();
        int j=0;
        for(int i=0; i<n; i++) 
            if (hasX[s[i]-'a']) s[j++] = s[i];
        s.resize(j);
        n=j;
        maxLen=n/k;
        
        for(int i=25; i>=0; i--)
            if (hasX[i]) chars.push_back('a'+i);
            
        string t="";
        dfs(s, t, k);
        return ans;
    }
};