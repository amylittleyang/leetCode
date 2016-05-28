# Binary Tree Right Side View

Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

For example:
Given the following binary tree,
```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
You should return [1, 3, 4].
```
### Solution
Beats 89% java submissions
``` java
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
    int lowestLevel=-1;
    List<Integer> list = new ArrayList<Integer>(); // the list to return
    public List<Integer> rightSideView(TreeNode root) {
        rightSideView(root,0); //populate list with right side view nodes
        return list;
    }
    public void rightSideView(TreeNode root, int myLevel){
        // traverse tree using preorder, for any node first recursively traverse its right subtree then its left subtree
        // myLevel is the level of root in the tree 
        // if a node is the first we see at any level, add it to the right side view. The nodes and their order must be correct given the order of this traversal
        if(root == null) return;
        if(myLevel>lowestLevel){
            list.add(root.val);
            lowestLevel = myLevel;
        }
        rightSideView(root.right,myLevel+1);
        rightSideView(root.left, myLevel+1);
    }
    
}
```
