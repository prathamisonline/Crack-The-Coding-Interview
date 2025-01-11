# Minimum Window Substring

## Problem
Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If no such substring exists, return the empty string `""`.

## Examples
1. **Input**: `s = "ADOBECODEBANC", t = "ABC"` → **Output**: `"BANC"`  
   The minimum window substring `"BANC"` includes 'A', 'B', and 'C' from string `t`.

2. **Input**: `s = "a", t = "a"` → **Output**: `"a"`  
   The entire string `s` is the minimum window.

3. **Input**: `s = "a", t = "aa"` → **Output**: `""`  
   The string `s` only contains one 'a', but two 'a's are required to satisfy `t`. Hence, the result is an empty string.

## Constraints
- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 10^5`
- `s` and `t` consist of uppercase and lowercase English letters.

## Follow-up
- Could you find an algorithm that runs in O(m + n) time?

```c++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> mpp;
        for (auto ele : t) {
            mpp[ele]++;
        }
        int i = 0;
        int j = 0;
        int count = mpp.size();
        int minCount = INT_MAX;
        int start=0;
        string ans = "";
        while (j < s.size()) {
            if (mpp.count(s[j]) > 0) {
                mpp[s[j]]--;
                if (mpp[s[j]] == 0) {
                    count--;
                }
            }
            if (count == 0) {
                while (count == 0) {
                    if (j - i + 1 < minCount) {
                        minCount = min(j - i + 1, minCount);
                        start=i;
                    }
                    if (mpp.count(s[i]) > 0) {
                        mpp[s[i]]++;
                        if (mpp[s[i]] > 0) {
                            count++;
                        }
                    }
                    i++;
                }
            }
            j++;
        }
        if(minCount==INT_MAX) return "";
        return s.substr(start,minCount);
    }
};
```