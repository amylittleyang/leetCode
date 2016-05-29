# Odd Even Linked List
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

Example:
Given ```1->2->3->4->5->NULL,```
return ```1->3->5->2->4->NULL.```

Note:
The relative order inside both the even and odd groups should remain as it was in the input. 
The first node is considered odd, the second node even and so on ...

## In Place Solution
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head==null || head.next == null || head.next.next==null) return head;
        ListNode last = head; // the last node in the linked list
        ListNode curr = head.next; // the pointer used to keep track of the even node being deleted
        ListNode pre = head; // points to the node before an even node
        int length=1; // length of the linked list
        while(last.next!=null){
            last=last.next;
            length+=1;
        }
        // when curr is at even node, delete curr using pre, the node before curr; append curr to last, upadate last;
        int count = 2; // the node number of curr
        while(count<=length){
            if(count%2==0){ // if curr is at an even node
                pre.next = curr.next;
                curr.next=null;
                last.next=curr;
                last=curr;
                curr=pre.next;
                count+=1;
            }else{ // if curr is at an odd node
                curr=curr.next;
                pre=pre.next;
                count+=1;
            }
        }
        return head;
    }
}
```

