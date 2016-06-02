# Product of Array Except Self
Given an array of n integers where n > 1, <code>nums</code>, return an array output such that <code>output[i]</code> is equal to the product of all the elements of nums except nums[i].

Solve it without division and in <code>O(n)</code>.

For example, given <code>[1,2,3,4]</code>, return <code>[24,12,8,6]</code>.

Follow up:
Could you solve it with constant space complexity? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)

### Solution
Create a new array to hold the products. First for loop fills the array[i] with nums[0]*nums[1]*...*nums[i-1]. Second for loop multiplies array[i]
with nums[i+1]*nums[i+2]*...*nums[n-1]. Solution runs in O(n) with constant space (excluding space used to create new array).
``` java
public class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        int left=nums[0];
        res[0]=1;
        for(int i=1;i<n;i++){
            res[i]=left;
            left*=nums[i];
        }
        int right=nums[n-1];
        for(int i=n-2;i>=0;i--){
            res[i]*=right;
            right*=nums[i];
        }
        return res;
    }
}
```

