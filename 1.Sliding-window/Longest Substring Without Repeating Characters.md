# Longest Substring Without Repeating Characters

## Problem
Find the length of the longest substring in `s` without repeating characters.

## Examples

### Example 1
**Input**: `s = "abcabcbb"`  
**Output**: `3` ("abc")

### Example 2
**Input**: `s = "bbbbb"`  
**Output**: `1` ("b")

### Example 3
**Input**: `s = "pwwkew"`  
**Output**: `3` ("wke")

## Constraints
- `0 <= s.length <= 50,000`
- `s` contains English letters, digits, symbols, and spaces.

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