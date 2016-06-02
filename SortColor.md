# Sort Color
Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.   

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.   

Note:   
You are not suppose to use the library's sort function for this problem.

Follow up:   
A rather straight forward solution is a two-pass algorithm using counting sort.   
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.

Could you come up with an one-pass algorithm using only constant space?

### In-place one-pass solution
Set two pointers <code>lastZero</code> and <code>firstTwo</code>. <code>lastZero</code> points to the first index that is not zero (all elements before it are 0); <code>firstTwo</code> points to the first index that is not two (all elements after it are 2).
Iterate through the region between the two pointers, if encounter a 2, swap it with <code>firstTwo</code>; if encounter a 0, swap it with <code>lastZero</code>. The two segments marked by the two pointers 
grow toward the middle of the array until all 0s and 2s are put into the respective segments, leaving all 1s gathered in the middle region.
``` java
public void sortColors(int[] nums) {
        int lastZero=0;// points to the first index that is not zero 
        int firstTwo=nums.length-1;// points to the last index that is not two
        // adjust lastZero to the right idx

        for(int i=0;i<nums.length;i++){
            if(nums[i]!=0){
                lastZero=i;
                break;
            }
        }
        if(lastZero==-1) return; // array only has 0s
        for(int i=nums.length-1;i>=0;i--){
            if(nums[i]!=2){
                firstTwo=i;
                break;
            }
        }
        if(firstTwo==-1) return;// array only has 2s.
        for(int i=lastZero;i<=firstTwo;i++){
            while(nums[i]==2 && i<firstTwo){
                // swap nums[i] with nums[firstTwo], decrement firstTwo, decrement i, return
                nums[i]=nums[firstTwo];
                nums[firstTwo]=2;
                firstTwo-=1;
            }
            while(nums[i]==0 && i>lastZero){
                // swap nums[i] with nums[lastZero], increment lastZero, decrement i, return
                nums[i] = nums[lastZero];
                nums[lastZero]=0;
                lastZero+=1;
            }
        }
    }
```
