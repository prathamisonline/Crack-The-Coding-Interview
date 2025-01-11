# Check if String Contains Permutation of Another String

## Problem
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1` as a substring, otherwise return `false`.

## Examples
1. **Input**: `s1 = "ab", s2 = "eidbaooo"` → **Output**: `true`  
   `"ba"` is a permutation of `"ab"` and is a substring of `s2`.
2. **Input**: `s1 = "ab", s2 = "eidboaoo"` → **Output**: `false`  
   No permutation of `"ab"` is a substring of `s2`.

## Constraints
- `1 <= s1.length, s2.length <= 10^4`
- `s1` and `s2` consist of lowercase English letters.

```c++
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        unordered_map<int, int> mpp;
        for (auto s : s1) {
            mpp[s]++;
        }
        int i = 0;
        int j = 0;
        int count = mpp.size();
        while (j < s2.size()) {
            if (mpp.find(s2[j]) != mpp.end()) {
                mpp[s2[j]]--;
                if (mpp[s2[j]] == 0) {
                    count--;
                }
            }

            if (count == 0) {
                while (count == 0) {
                    if (j - i + 1 == s1.size()) {
                        return true;
                    }
                    if (mpp.count(s2[i]) > 0) {
                        mpp[s2[i]]++;
                        if (mpp[s2[i]] > 0) {
                            count++;
                        }
                    }
                    i++;
                }
            }
            j++;
        }
        return false;
    }
};
```