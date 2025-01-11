# Longest Substring with Same Letters After k Replacements

## Problem
Given a string `s` and an integer `k`, replace up to `k` characters in `s` to get the longest substring with identical characters.

## Examples
1. **Input**: `s = "ABAB", k = 2` → **Output**: `4`  
   Replace two 'A's with 'B's to get `"BBBB"`.  
2. **Input**: `s = "AABABBA", k = 1` → **Output**: `4`  
   Replace one 'A' to get `"AABBBBA"` → `"BBBB"` is of length `4`.

## Constraints
- `1 <= s.length <= 10^5`
- `s` has uppercase English letters only.
- `0 <= k <= s.length`

```c++
class Solution {
public:
    int characterReplacement(string s, int k) {
        unordered_map<char, int> mpp;
        int i = 0;
        int j = 0;
        int maxLen = INT_MIN;
        int maxCount = 0;
        while (j < s.size()) {
            mpp[s[j]]++;

            maxCount = max(maxCount, mpp[s[j]]);

            if (j - i + 1 - maxCount <= k) {
                maxLen = max(maxLen, j - i + 1);
            } else {
                    mpp[s[i]]--;
                    i++;
                  
            }
            j++;
        }
        return maxLen == INT_MIN ? 0 : maxLen;
    }
};
```