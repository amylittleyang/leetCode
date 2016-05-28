# Binary Search Tree Iterator
### Problem
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

### Solution
Beats 80.53% of java submissions ✌️
``` java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class BSTIterator {
    private List<TreeNode> stack = new ArrayList<TreeNode>();
    public BSTIterator(TreeNode root) {
        // initialize stack with path to leftmost node, including the leftmost node
        while(root!=null){
            stack.add(0,root);
            root=root.left;
        }
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        // return if stack is empty
        return !stack.isEmpty();
    }

    /** @return the next smallest number */
    public int next() {
        // return the top of the stack T
        // pop T from the stack
        // push the path from T to leftmost node in T's right subtree to the stack
        TreeNode top = stack.remove(0);
        TreeNode curr = top.right;
        while(curr!=null){
            stack.add(0,curr);
            curr=curr.left;
        }
        return top.val;
    }
}

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = new BSTIterator(root);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

