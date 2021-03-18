## 一般链表都需要增加一个哑结点，前驱结点和后继节点

```Java
ListNode dummy=new ListNode(0);
        dummy.next=head;                                    
        ListNode pre=dummy;
```

### 递归反转链表

```Java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }

    // 调用递推公式反转当前结点之后的所有节点
    // 返回的结果是反转后的链表的头结点
    ListNode newHead = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}

```
