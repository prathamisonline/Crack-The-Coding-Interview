Given a binary array nums and an integer goal, return the number of non-empty subarrays with a sum goal.

A subarray is a contiguous part of the array.

Example 1:

Input: nums = [1,0,1,0,1], goal = 2
Output: 4
Explanation: The 4 subarrays are bolded and underlined below:
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
Example 2:

Input: nums = [0,0,0,0,0], goal = 0
Output: 15
 

Constraints:

1 <= nums.length <= 3 * 104
nums[i] is either 0 or 1.
0 <= goal <= nums.length



```c++
class Solution {
public:
    int numSubarraysWithSum(vector<int>& nums, int goal) {
        return atMost(nums, goal) - atMost(nums, goal - 1);
    }

    int atMost(vector<int>& nums, int goal) {
        int tail = 0, sum = 0, result = 0;

        for (int head = 0; head < nums.size(); head++) {
            sum += nums[head];

            // Move the tail pointer to maintain the sum constraint
            while (sum > goal && tail <= head) {
                sum -= nums[tail];
                tail++;
            }

            // Add the number of subarrays with a sum <= goal
            result += head - tail + 1;
        }

        return result;
    }
};
```