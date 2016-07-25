# House Robber
### Problem
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

### Solution
Dynamic programming solution. <code>sum[i]</code> is the maximum sum one can rob from house 0 to house i. Note the following relation: <code>sum[i] = Math.max(sum[i-2]+nums[i],sum[i-1])</code>.   
``` java
public class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        if(nums.length == 1) {
            return nums[0];
        }
        int[] sum = new int[nums.length];
        sum[0] = nums[0];
        sum[1] = Math.max(nums[0],nums[1]); 
        for(int i=2;i<nums.length;i++) {
            sum[i] = Math.max(sum[i-2]+nums[i],sum[i-1]);
        }
        return sum[nums.length-1];
    }
}
```
