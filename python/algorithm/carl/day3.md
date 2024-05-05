# 代码随想录算法训练营第三天|203.移除链表元素|707.设计链表|206.反转链表

## 链表基础

[链表基础](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 203. 移除链表元素

[题目链接](https://leetcode-cn.com/problems/remove-linked-list-elements/)
```py
from typing import *


class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def removeElements(self, val: int, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        cur = dummy_head
        while cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur = cur.next
        return dummy_head.next


def main():
    s = Solution()
    head = ListNode(1, ListNode(2, ListNode(6, ListNode(3, ListNode(4, ListNode(5, ListNode(6)))))))
    res = s.removeElements(5, head)
    while res:
        print(res.val, end=" ")
        res = res.next


if __name__ == "__main__":
    main()

```

## 707. 设计链表

[题目链接](https://leetcode-cn.com/problems/design-linked-list/)
```py
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class MyLinkedList:
    def __init__(self):
        self.dummy_head = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        cur = self.dummy_head.next
        for i in range(index):
            cur = cur.next
        return cur.val

    def addAtHead(self, val: int) -> None:
        self.dummy_head.next = ListNode(val, self.dummy_head.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        cur = self.dummy_head
        while cur.next:
            cur = cur.next
        cur.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        cur = self.dummy_head
        if index > self.size or index < 0:
            return
        for i in range(index):
            cur = cur.next
        cur.next = ListNode(val, cur.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index >= self.size or index < 0:
            return
        cur = self.dummy_head
        for i in range(index):
            cur = cur.next
        cur.next = cur.next.next
        self.size -= 1


def main():
    myLinkedList = MyLinkedList()
    print(myLinkedList.addAtHead(1))
    print(myLinkedList.addAtTail(3))
    print(myLinkedList.addAtIndex(1, 2))
    print(myLinkedList.get(1))
    print(myLinkedList.deleteAtIndex(1))
    print(myLinkedList.get(1))


if __name__ == "__main__":
    main()
```

## 206. 反转链表

[题目链接](https://leetcode-cn.com/problems/reverse-linked-list/)
```py
from typing import Optional


class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur = head
        pre = None
        while cur:
            temp = cur.next
            cur.next = pre
            pre = cur
            cur = temp
        return pre


def main():
    s = Solution()
    head = ListNode(1, ListNode(2, ListNode(6, ListNode(3, ListNode(4, ListNode(5, ListNode(6)))))))
    reversed_head = s.reverseList(head)
    while reversed_head:
        print(reversed_head.val, end=" ")
        reversed_head = reversed_head.next


if __name__ == "__main__":
    main()
```
