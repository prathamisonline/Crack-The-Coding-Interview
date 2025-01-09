# 904. Fruit Into Baskets

## Problem Description
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the type of fruit the `i-th` tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

1. You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
2. Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the starting tree) while moving to the right. The picked fruits must fit in one of your baskets.
3. Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return the **maximum number of fruits** you can pick.

---

## Examples

### Example 1:
**Input:**  
`fruits = [1, 2, 1]`  

**Output:**  
`3`  

**Explanation:**  
We can pick from all 3 trees.

---

### Example 2:
**Input:**  
`fruits = [0, 1, 2, 2]`  

**Output:**  
`3`  

**Explanation:**  
We can pick from trees `[1, 2, 2]`.  
If we had started at the first tree, we would only pick from trees `[0, 1]`.

---

### Example 3:
**Input:**  
`fruits = [1, 2, 3, 2, 2]`  

**Output:**  
`4`  

**Explanation:**  
We can pick from trees `[2, 3, 2, 2]`.  
If we had started at the first tree, we would only pick from trees `[1, 2]`.

---

## Constraints:



- `1 <= fruits.length <= 10^5`
- `0 <= fruits[i] < fruits.length`
```class Solution {
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
};```