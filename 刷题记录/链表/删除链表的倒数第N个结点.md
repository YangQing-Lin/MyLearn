#### <a href="https://leetcode.cn/problems/remove-nth-node-from-end-of-list/">删除链表的倒数第N个结点</a>

---------

链表的题目

凡是可能涉及到头结点的操作，都可以设一个虚拟头结点dummy

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        int s = 0;
        for (ListNode p = dummy; p != null; p = p.next) s ++;

        ListNode p = dummy;
        for (int i = 0; i < s - n - 1; i ++) p = p.next;
        p.next = p.next.next;
        
        return dummy.next;
    }
}
```

