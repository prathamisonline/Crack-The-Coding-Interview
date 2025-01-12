# Find All Anagram Indices in String

## Problem
Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`. The order of indices can vary.

## Examples
1. **Input**: `s = "cbaebabacd", p = "abc"` → **Output**: `[0, 6]`  
   The substrings `"cba"` and `"bac"` are anagrams of `"abc"`, starting at indices 0 and 6, respectively.
2. **Input**: `s = "abab", p = "ab"` → **Output**: `[0, 1, 2]`  
   The substrings `"ab"`, `"ba"`, and `"ab"` are anagrams of `"ab"`, starting at indices 0, 1, and 2.

## Constraints
- `1 <= s.length, p.length <= 3 * 10^4`
- `s` and `p` consist of lowercase English letters.

```c++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        unordered_map<char, int> mpp;
        for (auto ele : p) {
            mpp[ele]++;
        }

        int i = 0;
        int j = 0;
        int count = mpp.size();
        vector<int> ans;
        while (j < s.size()) {
            if (mpp.count(s[j]) > 0) {
                mpp[s[j]]--;
                if (mpp[s[j]] == 0) {
                    count--;
                }
            }
            if (count == 0) {
                while (count == 0) {
                    if (j - i + 1 == p.size()) {
                        ans.push_back(i);
                    }
                    if(mpp.count(s[i])>0){
                        mpp[s[i]]++;
                        if(mpp[s[i]]>0){
                            count++;
                        }
                    }
                    i++;
                }
            }
            j++;
        }
        return ans;
    }
};
```