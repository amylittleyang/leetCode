# Partition List
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,
Given ```1->4->3->2->5->2``` and x = 3,
return ```1->2->2->4->3->5```.

## Solution
Keep two pointers: one for the last node where all nodes before it are smaller than x, another pointer looking for any node after the first pointer but with value smaller than x.
Solution done in one pass with constant memory.
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
    public ListNode partition(ListNode head, int x) {
        if(head==null||head.next==null) return head;
        ListNode dummyHead = new ListNode(x);
        dummyHead.next = head;
        head = dummyHead;
        ListNode endSmallerThan = head; // the node that switches from <x to >=x
        ListNode ptr = head;// the node scanning for node <=x after endSmallerThan
        // adjust endSmallerThan to the first node where the list switches from <x to >=x
        while(true){
            if(endSmallerThan.next==null){
                // all nodes are already in order
                return head.next; // skip dummy
            }
            if(endSmallerThan.next !=null && endSmallerThan.next.val>=x){
                break; // found the cutoff node
            }
            endSmallerThan=endSmallerThan.next;
        }
        // Note endSmallerThan.next !=null
        ptr=endSmallerThan.next;
        ListNode pre=endSmallerThan; // point to node before ptr to delete node <x
        while(ptr!=null){
            if(ptr.val<x){
                // delete node from list
                pre.next=ptr.next;
                // insert node to endSmallerThan
                ListNode nextNode = endSmallerThan.next;
                ptr.next=nextNode;
                endSmallerThan.next=ptr;
                endSmallerThan=ptr;
                ptr=pre.next;
            }else{
                ptr=ptr.next;
                pre=pre.next;
            }
        }
        return head.next; // skip dummy
    }
}
```
## A (Far) More Concise Solution
Separate the list into two distinct lists and link them up afterwards. Solution credit to <a href="https://leetcode.com/discuss/user/shichaotan">@shichaotan</a>
``` c
ListNode *partition(ListNode *head, int x) {
    ListNode node1(0), node2(0);
    ListNode *p1 = &node1, *p2 = &node2;
    while (head) {
        if (head->val < x)
            p1 = p1->next = head;
        else
            p2 = p2->next = head;
        head = head->next;
    }
    p2->next = NULL;
    p1->next = node2.next;
    return node1.next;
}
```
