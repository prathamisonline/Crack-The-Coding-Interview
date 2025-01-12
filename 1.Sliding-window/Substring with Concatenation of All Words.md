# Problem Description

Given a string `s` and an array of strings `words` (all of the same length), find all starting indices of substrings in `s` that are concatenations of all `words` in any permutation. Return the indices in any order.

### Example 1
**Input**: `s = "barfoothefoobarman"`, `words = ["foo", "bar"]`  
**Output**: `[0, 9]`  
**Explanation**: Substrings `"barfoo"` and `"foobar"` are concatenations of `["bar", "foo"]`.

### Example 2
**Input**: `s = "wordgoodgoodgoodbestword"`, `words = ["word", "good", "best", "word"]`  
**Output**: `[]`  
**Explanation**: No valid concatenated substring.

### Example 3
**Input**: `s = "barfoofoobarthefoobarman"`, `words = ["bar", "foo", "the"]`  
**Output**: `[6, 9, 12]`  
**Explanation**: Valid substrings are `"foobarthe"`, `"barthefoo"`, and `"thefoobar"`.





# Problem: Find Concatenated Substrings

## Problem Explanation

Given:  
- A string `s` of length `n`.  
- An array of strings `words`, where each word has length `w` and the array contains `m` words.  

We need to find all starting indices `i` in `s` such that the substring `s[i, i + (m × w)]` can be split into `m` words of length `w`, which are a permutation of `words`.  

---

## Approach

### Subproblem Definition
Define a subproblem `(i)` as whether index `i` is a valid starting index. For example, to solve for `i = 0`:  
- Extract the substring `s[i, i + (m × w)]`.  
- Split it into `m` words of length `w`.  
- Check if these words match any permutation of `words`.  

### Brute Force Approach
1. Create a hashmap for `words` to store their frequency.  
2. For each index `i` in `s`, generate another hashmap to track observed words in `s[i, i + (m × w)]`.  
3. If the hashmaps match, add `i` to the result.  

**Time Complexity**:  
- Solving a single subproblem `(i)` takes **O(m × w)**.  
- Checking all possible indices `(0 to n-1)` takes **O(n × (m × w))**.  
- **Total Time Complexity**: **O(n × m × w)** (cubic complexity).

---

## Optimization Using Sliding Window

### Key Insight
Re-use computations from previously solved subproblems.  
- When solving `(i)`, a hashmap of observed words is created.  
- For `(i + w)`, the same hashmap can be reused by:  
  - Removing the first word of length `w` starting at `i`.  
  - Adding the next word of length `w` starting at `i + (m × w)`.  

This reuse of computations effectively creates a **sliding window** of `m × w` characters.

### Optimized Steps
1. Divide `s` into `w` independent groups:  
   - For each group, start solving subproblems from indices `i, i+w, i+2w, ...`.  
   - Use the sliding window approach to reuse computations.  

**Time Complexity**:  
- Each group takes **O(n)** to solve all subproblems.  
- There are `w` groups.  
- **Total Time Complexity**: **O(n × w)**.

---

## Sliding Window Visualization
### For Each Group
Group 1:  
`i → i+w → i+2w → i+3w → ...`  

Group 2:  
`i+1 → (i+1)+w → (i+1)+2w → (i+1)+3w → ...`  

Group 3:  
`i+2 → (i+2)+w → (i+2)+2w → (i+2)+3w → ...`  

...

Group `w`:  
`i+(w-1) → i+(w-1)+w → i+(w-1)+2w → ...`  

---

## Final Complexity Analysis
- Each group uses **O(n)** time with sliding window.  
- There are `w` groups.  
- **Overall Time Complexity**: **O(n × w)**.  
- This is much more efficient than the brute force approach (**O(n × m × w)**).

---

## Summary
This problem is solved by combining the **sliding window** technique and the reuse of hashmaps across subproblems. The optimization reduces the complexity to **O(n × w)** by processing `w` sliding windows independently.



```c++
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> result;
        if (words.empty() || s.empty())
            return result;

        unordered_map<string, int> wordCount;
        int wordLength = words[0].size();
        int numWords = words.size();
        int windowLength = wordLength * numWords;

        // Build the frequency map for words
        for (const auto& word : words) {
            wordCount[word]++;
        }

        // Traverse each possible starting point within the word length
        for (int i = 0; i < wordLength; ++i) {
            unordered_map<string, int> windowMap;
            int left = i, right = i, count = 0;

            while (right + wordLength <= s.size()) {
                string word = s.substr(right, wordLength);
                right += wordLength;

                if (wordCount.find(word) != wordCount.end()) {
                    windowMap[word]++;
                    if (windowMap[word] <= wordCount[word]) {
                        count++;
                    }

                    while (windowMap[word] > wordCount[word]) {
                        string leftWord = s.substr(left, wordLength);
                        left += wordLength;
                        windowMap[leftWord]--;
                        if (windowMap[leftWord] < wordCount[leftWord]) {
                            count--;
                        }
                    }

                    if (count == numWords) {
                        result.push_back(left);
                    }
                } else {
                    // Reset the window if the word is not in words
                    windowMap.clear();
                    count = 0;
                    left = right;
                }
            }
        }

        return result;
    }
};
```