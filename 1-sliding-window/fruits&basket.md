# 904. Fruit Into Baskets

## Problem Description
You are visiting a farm with a row of fruit trees, represented by an array `fruits` where `fruits[i]` is the type of fruit on the `i-th` tree. You have **two baskets**, each holding one type of fruit (unlimited quantity). Starting from any tree, pick one fruit from every tree moving right until encountering a fruit that doesn’t fit in your baskets.

Return the **maximum number of fruits** you can pick.

---

## Examples

- **Input:** `fruits = [1, 2, 1]`  
  **Output:** `3`  
  **Explanation:** Pick all 3 trees.

- **Input:** `fruits = [0, 1, 2, 2]`  
  **Output:** `3`  
  **Explanation:** Pick trees `[1, 2, 2]`.

- **Input:** `fruits = [1, 2, 3, 2, 2]`  
  **Output:** `4`  
  **Explanation:** Pick trees `[2, 3, 2, 2]`.

---

## Constraints
- `1 <= fruits.length <= 10^5`
- `0 <= fruits[i] < fruits.length`

```c++

class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        unordered_map<int, int> mpp;
        int i = 0;
        int j = 0;
        int maxLen = INT_MIN;
        int n = fruits.size();
        while (j < n) {
            mpp[fruits[j]]++;
            if (mpp.size() <= 2) {
                maxLen = max(maxLen, j - i + 1);
            } else if (mpp.size() > 2) {
                while (mpp.size() > 2) {
                    mpp[fruits[i]]--;
                    if (mpp[fruits[i]] == 0) {
                        mpp.erase(fruits[i]);
                    }
                    i++;
                }
            }
            j++;
        }
        return maxLen == INT_MIN ? 0 : maxLen;
    }
}
```