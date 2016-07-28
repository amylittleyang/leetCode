#Unique Binary Search Trees
### Problem
Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

### Solution
For a tree with <code>n</code> nodes. Fix any node <code>M</code> as root, let the total number of nodes to the left of <code>M</code> be <code>x</code>, and let total number of 
nodes to the right of <code>M</code> be <code>n-x-1</code>. Then the number of unique BST rooted at <code>M</code> is <code>numTrees(x)</code> * <code>numTrees(n-x-1)</code> by the product rule.
```java
public class Solution {
    public int numTrees(int n) {
        int[] nums = new int[n+1];
        nums[0] = 1;
        nums[1] = 1;
        for(int i=2;i<=n;i++) {
            int sum=0;
            for(int j=0;j<i;j++) {
                sum+=nums[j]*nums[i-j-1];
            }
            nums[i] = sum;
        }
        return nums[n];
    }
}
```
