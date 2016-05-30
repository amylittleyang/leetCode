# Kth Smallest Element in a BST
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note: 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.
## Recursive Solution
Beats 95% of java submissions
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
    private int myIdx = 1; // index of node being traversed in an inorder traversal of the tree
    public int kthSmallest(TreeNode root, int k) {
        // return -1 if node at index k does not exist in subtree at root
        // return node.val if found a node at index k in the subtree at root
        if(root==null) return -1;
        int node = kthSmallest(root.left, k); // check if node exists in left subtree of root
        if(node!=-1) return node;
        if(myIdx==k) return root.val; // process the current node if node at idx k dne in left subtree
        myIdx+=1; // index of the next node that will be traversed in this inorder traversal
        node = kthSmallest(root.right,k); 
        // if the requested node is not in the right subtree of root as well -> node is not in the entire subtree at root -> should return -1
        return node;
    }
}
```
