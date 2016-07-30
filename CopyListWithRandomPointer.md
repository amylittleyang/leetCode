#Copy List with Random Pointer
###Problem
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.
###Solution
Below is a one-pass solution with ```O(n)``` time complexity and ```O(n)``` space complexity where ```n``` is the number of nodes in the list.
 Use a ```HashMap``` to pair up the original list node with the copied nodes. On visiting a node, check if the copy of ```node.next``` and 
```node.random``` already exist in the HashMap. If they exist, connect the copies to the node's copy; else create the copies and connect them to 
node's copy.   

We could have a cleaner implementation using javascript, since we can just store the one-to-one relationship between a node and its copy
 as an extra property of a node at runtime, saving us the trouble of maintaining a hash map.

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        Map<RandomListNode,RandomListNode> copyMap = new HashMap<RandomListNode,RandomListNode>();
        if(head == null) {
            return null;
        }
        RandomListNode curr = head;
        RandomListNode copyHead = new RandomListNode(curr.label);
        copyMap.put(head,copyHead);
        while(curr!=null) {
            RandomListNode random = curr.random;
            // set up random pointer of copied node
            if(random == null) {
                copyMap.get(curr).random = null;
            } else {
                if(!copyMap.containsKey(random)) {
                    RandomListNode copyRandom = new RandomListNode(random.label);
                    copyMap.put(random,copyRandom);
                }
                copyMap.get(curr).random = copyMap.get(curr.random);
            }
            // copy node for curr.next
            if(curr.next!=null) {
                if(copyMap.containsKey(curr.next)) {
                    copyMap.get(curr).next = copyMap.get(curr.next);
                } else {
                    copyMap.put(curr.next, new RandomListNode(curr.next.label));
                    copyMap.get(curr).next = copyMap.get(curr.next);
                }
            }
            curr=curr.next;
        }       
        return copyHead;
    }
}
```
