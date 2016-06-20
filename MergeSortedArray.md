# Merge Sorted Array

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.   

Note:   
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

## Solution  
In-place one-pass solution.   

``` java
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int idx1 = m-1;
        int idx2 = n-1;
        int topPtr = m+n-1;
        while(idx2>=0) {
            int one;
            int two = nums2[idx2];
            if (idx1<0) {
                one = Integer.MIN_VALUE;
            } else {
                one = nums1[idx1];
            }
            if(one>=two) {
                nums1[topPtr] = one;
                idx1--;
                topPtr--;
            } else {
                nums1[topPtr] = nums2[idx2];
                idx2--;
                topPtr--;
            }
        }
    }
}
```
