#Validate Binary Search Tree
### Problem
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
Example 1:
```
    2
   / \
  1   3
```
Binary tree ```[2,1,3]```, return ```true```.
Example 2:
```
    1
   / \
  2   3
```
Binary tree ```[1,2,3]```, return ```false```.
### Solution
Use inorder traversal to compare tree nodes in (supposedly) sorted order.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class Solution {
    int lastNode;
    TreeNode leftmost;
    public boolean isValidBST(TreeNode root) {
        TreeNode curr = root;
        while(curr!=null) {
            if(curr.left==null) {
                lastNode = curr.val;
                leftmost = curr;
            }
            curr = curr.left;
        }
        return validateBST(root);
    }
    public boolean validateBST(TreeNode root) {
        if(root==null){
            return true;
        }
        if(root.left==null && root.right==null) {
            if(root!=leftmost && !(lastNode<root.val)) {
                return false;
            }
            lastNode = root.val;
            return true;
        }
        if(!validateBST(root.left)) {
            return false;
        }
        if(root!=leftmost && !(lastNode<root.val)) {
            return false;
        }
        lastNode = root.val;
        if(!validateBST(root.right)) {
            return false;
        }
        return true;
    }
    
}
```
