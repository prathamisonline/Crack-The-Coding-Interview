# 3. Longest Substring Without Repeating Characters

## Problem Description
Given a string `s`, find the length of the **longest substring** without repeating characters.

---

## Examples

- **Input:** `s = "abcabcbb"`  
  **Output:** `3`  
  **Explanation:** The substring is `"abc"`, with length 3.

- **Input:** `s = "bbbbb"`  
  **Output:** `1`  
  **Explanation:** The substring is `"b"`, with length 1.

- **Input:** `s = "pwwkew"`  
  **Output:** `3`  
  **Explanation:** The substring is `"wke"`, with length 3.  
  Note: `"pwke"` is not valid because it is a subsequence, not a substring.

---

## Constraints
- `0 <= s.length <= 5 * 10^4`
- `s` consists of English letters, digits, symbols, and spaces.
  
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int i = 0;
        int j = 0;
        int maxLen = INT_MIN;
        unordered_map<char, int> mpp;
        int count = 0;
        while (j < s.size()) {
            mpp[s[j]]++;
            count++;
            if (mpp.size() == count) {
                maxLen=max(maxLen,count);
                j++;
            } else if (mpp.size() < count) {
                while (mpp.size() < count) {
                    mpp[s[i]]--;
                    if (mpp[s[i]] == 0) {
                        mpp.erase(s[i]);
                    }
                    count--;
                    i++;
                }
                j++;
            }
        }
        return maxLen == INT_MIN ? 0 : maxLen;
    }
};
```