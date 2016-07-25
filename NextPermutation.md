# Next Permutation
### Problem
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
<code>1,2,3 → 1,3,2</code>   <br>
<code>3,2,1 → 1,2,3</code>   <br>
<code>1,1,5 → 1,5,1</code>   <br>

### Solution
In-place O(n) solution.
``` java
    public void nextPermutation(int[] nums) {
        int reorderFrom = -1;
        // traverse nums from right to left, find the first index where nums[index] < nums[index+1]
        for(int i=nums.length-2;i>=0;i--) {
            if(nums[i]<nums[i+1]) {
                reorderFrom = i;
                break;
            }
        }
        reorder(nums, reorderFrom); // reorderFrom = -1 if nums monotonically decreases
    }
    public void reorder(int[] nums, int idx) {
        /* 
          starting from idx, numbers in the array monotonically decreases, 
          reverse this part of the array so that the array monotonically increases starting from idx
        */
        int start = idx+1;
        int end = nums.length-1;
        for(int i=start;i<=(start+end)/2;i++) {
            int temp = nums[i];
            nums[i] = nums[end-(i-start)];
            nums[end-(i-start)] = temp;
        }
        // return the reversed nums if nums is already the largest permutation
        if(idx == -1) {
            return;
        }
        // exchange nums[idx] with the smallest number in nums[idx+1,end]
        for(int i=idx;i<=nums.length-1;i++) {
            if(nums[idx]<nums[i]) {
                int temp = nums[idx];
                nums[idx] = nums[i];
                nums[i] = temp;
                break;
            }
        }
    }
```

To generate the next permutation lexicongraphically, we traverse <code>nums</code> from the least significant digit to the most significant digit. 
 If we find some index such that <code>nums[index] < nums[index+1]</code>, then permuting the subarray <code>nums[index, end]</code> and concatenating it with 
 <code>nums[0,index]</code> should give the next permutation.    
 <br>
 Therefore the problem becomes permuting <code>nums[index,end]</code> to be the next greater permutation.   
 <br>
 For example let <code>nums = [3,2,2,6,3,3,2,2,1]</code>. The next permutation will be <code>[3,2] + nextGreaterPermutation([2,6,3,3,2,2,1])</code>.   
 <br>
 To permute the subarray <code>nums[index,end]</code> in-place, the only operation allowed is swapping indices. 
 <br>We have a nice property: the subarray <code>nums[index+1,end]</code> monotonically decreases. Since <code>index</code> is the first decrease when we traverse the array from right to left,
  all elements to its right must be decreasing. For example <code>nums = [3,2,2,6,3,3,2,2,1]</code>, <code>index=2</code>, and the subarray [6,3,3,2,2,1] decreases.   
  <br>
  Using this property, we can easily find the next greater permutation of the subarray.    <br>
  Simply reverse the subarray <code>nums[index+1,end]</code> and swap <code>nums[index]</code> with the smallest element in <code>nums[index+1,end]</code> greater than it.    
  <br>
  In the above example, we first reverse [3,2,2,<b>6,3,2,2,1</b>] to
    [3,2,2,<b>1,2,2,3,3,6</b>]. Then swap the 2 with 3 [3,2,<b>2</b>,1,2,2,<b>3</b>,3,6] to get the next permutation [3,2,3,1,2,2,2,3,6].

