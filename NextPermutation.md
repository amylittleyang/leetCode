# Next Permutation
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
```1,2,3 → 1,3,2```   <br>
```3,2,1 → 1,2,3```   <br>
```1,1,5 → 1,5,1```   <br>

## Solution
To generate the next permutation lexicongraphically, we traverse ```nums``` from the least significant digit to the most significant digit. 
 If we find some index such that ```nums[index] < nums[index+1]```, then permuting the subarray ```nums[index, end]``` and concatenating it with 
 ```nums[0,index]``` should give the next permutation.    
 <br>
 Therefore the problem becomes permuting ```nums[index,end]``` to be the next greater permutation.   
 <br>
 For example let ```nums = [3,2,2,6,3,3,2,2,1]```. The next permutation will be ```[3,2] + nextGreaterPermutation([2,6,3,3,2,2,1])```.   
 <br>
 To permute the subarray ```nums[index,end]``` in-place, the only operation allowed is swapping indices. 
 <br>We have a nice property: the subarray ```nums[index+1,end]``` monotonically decreases. Since ```index``` is the first decrease when we traverse the array from right to left,
  all elements to its right must be decreasing. For example ```nums = [3,2,2,6,3,3,2,2,1]```, ```index=2```, and the subarray [6,3,3,2,2,1] decreases.   
  <br>
  Using this property, we can easily find the next greater permutation of the subarray.    <br>
  Simply reverse the subarray ```nums[index+1,end]``` and swap ```nums[index]``` with the smallest element in ```nums[index+1,end]``` greater than it.    
  <br>
  In the above example, we first reverse [3,2,2,<b>6,3,2,2,1</b>] to
    [3,2,2,<b>1,2,2,3,3,6</b>]. Then swap the 2 with 3 [3,2,<b>2</b>,1,2,2,<b>3</b>,3,6] to get the next permutation [3,2,3,1,2,2,2,3,6].

    
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
    
