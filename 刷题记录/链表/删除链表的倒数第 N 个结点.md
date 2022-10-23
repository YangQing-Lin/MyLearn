#### <a href="https://leetcode.cn/problems/remove-nth-node-from-end-of-list/">删除链表的倒数第 N 个结点</a>

------------

可能会删除头节点，所以创建一个虚拟头节点

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

        int sum = 0;
        for (ListNode i = dummy; i != null; i = i.next) {
            sum ++;
        }

        int t = sum - n;
        for (ListNode i = dummy; i != null; i = i.next) {
            t --;
            if (t == 0) {
                i.next = i.next.next;
            }
        }
        return dummy.next;
    }
}
```

