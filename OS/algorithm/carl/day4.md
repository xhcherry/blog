# 代码随想录算法训练营第三天|24.两两交换链表中的节点|19.删除链表的倒数第N个节点|160.链表相交|142.环形链表II

## 24.两两交换链表中的节点

[题目链接](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)



```py
from typing import Optional


class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        cur = dummy_head
        while cur.next.next and cur.next:
            tmp = cur.next
            tmp1 = cur.next.next.next
            cur.next = cur.next.next
            cur.next.next = tmp
            tmp.next = tmp1
            cur = cur.next.next
        return dummy_head.next


def main():
    s = Solution()
    head = ListNode(1, ListNode(2, ListNode(6, ListNode(3, ListNode(4, ListNode(5, ListNode(6)))))))
    res = s.swapPairs(head)
    while res:
        print(res.val, end=" ")
        res = res.next


if __name__ == "__main__":
    main()

```

## 19.删除链表的倒数第N个节点

[题目链接](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)



```py

```

## 160.链表相交

[题目链接](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)



```py

```

## 142.环形链表II

[题目链接](https://leetcode-cn.com/problems/linked-list-cycle-ii/)



```py

```