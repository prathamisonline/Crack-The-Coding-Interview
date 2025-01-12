# Max Consecutive 1's After k Flips

## Problem
Given a binary array `nums` and an integer `k`, return the maximum number of consecutive 1's after flipping at most `k` 0's.

## Examples
1. **Input**: `nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2` → **Output**: `6`  
   Flip two 0's to get the longest subarray `[1,1,1,0,0,1,1,1,1,1,1]`.
2. **Input**: `nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3` → **Output**: `10`  
   Flip three 0's to get the longest subarray `[0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]`.

## Constraints
- `1 <= nums.length <= 10^5`
- `nums[i]` is either 0 or 1.
- `0 <= k <= nums.length`

```c++
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int i = 0;
        int j = 0;
        int maxLen = INT_MIN;
        int maxCount = 0;
        while (j < nums.size()) {
            if(nums[j]==0){
                maxCount++;
            }
            if(maxCount<=k){
                maxLen=max(maxLen,j-i+1);
            }else{
                while(maxCount>k){
                    if(nums[i]==0){
                        maxCount--;
                    }
                    i++;
                }
            }
            j++;
            
        }
        return maxLen == INT_MIN ? 0 : maxLen;
    }
};
```