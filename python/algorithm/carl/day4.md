# 代码随想录算法训练营第4天|24.两两交换链表中的节点|19.删除链表的倒数第N个节点|160.链表相交|142.环形链表II

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
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy_head = ListNode(next=head)
        fast = slow = dummy_head
        for i in range(n + 1):
            fast = fast.next
        while fast:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return dummy_head.next


def main():
    s = Solution()
    head = ListNode(1, ListNode(2, ListNode(6, ListNode(3, ListNode(4, ListNode(5, ListNode(6)))))))
    res = s.removeNthFromEnd(head, 3)
    while res:
        print(res.val, end=" ")
        res = res.next


if __name__ == '__main__':
    main()

```

## 160.链表相交

[题目链接](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```py
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        lenA = lenB = 0
        cur = headA
        while cur:
            lenA += 1
            cur = cur.next
        cur = headB
        while cur:
            lenB += 1
            cur = cur.next
        curA, curB = headA, headB
        if lenA > lenB:
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA
        for i in range(lenB - lenA):
            curB = curB.next
        while curA:
            if curA == curB:
                return curA
            else:
                curA = curA.next
                curB = curB.next
        return None


def main():
    s = Solution()
    # 创建链表1: 1 -> 2 -> 3 -> 4 -> 5
    list1 = ListNode(1)
    list1.next = ListNode(2)
    list1.next.next = ListNode(3)
    list1.next.next.next = ListNode(4)
    list1.next.next.next.next = ListNode(5)

    # 创建链表2: 6 -> 7 -> 8 -> 9 -> 10 -> 4 -> 5
    list2 = ListNode(6)
    list2.next = ListNode(7)
    list2.next.next = ListNode(8)
    list2.next.next.next = ListNode(9)
    list2.next.next.next.next = ListNode(10)
    list2.next.next.next.next.next = list1.next.next.next  # 连接到共同的部分

    res = s.getIntersectionNode(headA, headB)
    print(res)


if __name__ == "__main__":
    main()

```

## 142.环形链表II

[题目链接](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```py
from typing import Optional


class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                slow = head
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
        return None


def main():
    # 创建带有循环的链表: 1 -> 2 -> 3 -> 4 -> 5 -> 2
    head = ListNode(1)
    head.next = ListNode(2)
    head.next.next = ListNode(3)
    head.next.next.next = ListNode(4)
    head.next.next.next.next = ListNode(5)
    head.next.next.next.next.next = head.next  # 创建循环

    s = Solution()
    res = s.detectCycle(head)

    print(res.val)


if __name__ == "__main__":
    main()

```