# Count Complete Tree Nodes
Given a complete binary tree, count the number of nodes.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

## Naive Solution
Not efficient. Takes O(n) time where n is the number of nodes in the tree.

``` java
 public int countNodes(TreeNode root) {
        if(root==null) return 0;
        return countNodes(root.left)+countNodes(root.right)+1;
    }
```

## Binary Search Solution
Also not efficient. Runtime almost the same as the naive solution. Thought it'd save time but didn't, since each different paths can have siginificant overlaps. Each nodes can be traversed multiple times.

``` java
private TreeNode myRoot;
    private int leftLevel;
    public int countNodes(TreeNode root) {
        if(root==null) return 0;
        if(root.left==null && root.right==null) return 1;
        myRoot = root;
        int maxLeft = levelInDir(0);
        leftLevel = maxLeft;
        //System.out.println("maxLeft="+maxLeft+"; maxRight="+maxRight);
        int numLastLevel = numberOfNodesAtLastLevel();
        int sum=(int)(Math.pow(2,maxLeft)-1);
        return sum+numLastLevel;
        
    }
    public int numberOfNodesAtLastLevel(){
        int lo=0;
        int hi=(int)(Math.pow(2,leftLevel)-1);
        if(exists(hi)){
            return hi+1;
        }
        while(lo!=hi){
            int mid = (lo+hi)/2;
            if(isHit(mid)){
                //System.out.println("numAtLastLevel="+(mid+1));
                return mid+1;
            }
            if(exists(mid)){
                lo=mid;
            }else{
                hi=mid;
            }
        }
        //System.out.println("error in BST code");
        return -1;
    }
    
    public int levelInDir(int dir){
        // dir==0 left length; dir==1 right length
        TreeNode curr = myRoot;
        int level=0;
        if(dir==0){
            // get max path length for the leftmost path
            while(curr!=null){
                level+=1;
                curr=curr.left;
            }
        }else{
            // get max path length for the rightmost path
            while(curr!=null){
                level+=1;
                curr=curr.right;
            }
        }
        return level-1;
    }
    public boolean exists(int idx){
        // return true if node at idx exists at the lowest level of the tree
        // turn idx into binary, a path in the tree starting from the root, 0 indicates going left, 1 indicates going right.
        String path = Integer.toBinaryString(idx);
        int diff = leftLevel-path.length();
        for(int i=0;i<diff;i++){
            path = "0"+path;
        }
        //System.out.println("path="+path);
        TreeNode curr = myRoot;
        for(int i=0;i<path.length();i++){
            char dir = path.charAt(i);
            if(dir == '0'){
                // turn left
                if(curr.left==null) return false; // curr gets to null before path, node DNE
                curr=curr.left;
            }else{
                // turn right
                if(curr.right==null) return false;// curr gets to null before path, node DNE
                curr=curr.right;
            }
        }
        return true;
    }
    public boolean isHit(int idx){
        // return true iff idx is the last node at lowest level of tree, idx+1 is the number of nodes in the lowest level
        return (exists(idx)==true) && (exists(idx+1)==false);
    }
```
## The real time-saving solution
A slight modificaiton to the naive solution. It turns out a quick check if the tree is balanced saves a lot of time at a lot of recursive calls.
``` java
public int countNodes(TreeNode root) {
        
        int leftDepth = leftDepth(root);
        int rightDepth = rightDepth(root);
        
        if (leftDepth == rightDepth)
            return (1 << leftDepth) - 1;
        else
            return 1+countNodes(root.left) + countNodes(root.right);

    }

    private int rightDepth(TreeNode root) {
        // TODO Auto-generated method stub
        int dep = 0;
        while (root != null) {
            root = root.right;
            dep++;
        }
        return dep;
    }
    
    private int leftDepth(TreeNode root) {
        // TODO Auto-generated method stub
        int dep = 0;
        while (root != null) {
            root = root.left;
            dep++;
        }
        return dep;
    }
```
solution credit to <a href="https://leetcode.com/discuss/user/mo10">@mo10</a>.
### Runtime Analysis
The above solution takes O(log(n)) to find the depth of the subtree at each recursion, where n is the number of nodes in the subtree at root. 
Each call to the function generates two subsequent recursive calls with input O(n/2). Since we're dealing with a complete binary tree, 
it's safe to conclude the number of nodes in the left subtree is roughly equal to the number of nodes in the right subtree.
Therefore the time complexity T(n) <= log(n)+2T(n/2) = log(n)+2*log(n/2)+4*log(n/8)... Note if the subtree is balanced there will be no recursive calls
on its side, so T(n) is upper bounded by the right hand side sum. Using some Math we can find the RHS < log(n)+2*log(n)+4*log(n)+...nlog(n)= 
log(n)*(1+2+4+...+n) = O(nlog(n)), a pretty nifty upper bound.
