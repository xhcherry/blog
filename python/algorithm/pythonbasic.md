# python数据结构

## 1.数组(Arrays)

Python没有内置数组类型，可以使用列表。数组是以相同名称保存的相同类型的值的集合。 数组中的每个值都被称为“元素”，索引表示其位置。

使用所需元素的索引调用数组名称来访问特定的元素。使用len()方法获取数组的长度。Python的数组在添加/减去元素时自动伸缩。

可以使用append()方法在现有数组的末尾添加一个额外的元素，而不是声明一个新数组。

Python中的数组可以包含任何类型的数据，包括整数、浮点数、字符串、布尔值等

使用列表(Lists)创建数组：my_array = [1, 2, 3, 4, 5]；使用my_array[0]访问第一个元素。

```python
cars = ["Toyota", "Tesla", "Hyundai"]
print(len(cars))
cars.append("Honda")
cars.pop(1)
for x in cars:
  print(x)
```

## 2.队列(Queue)

队列是一种线性数据结构，以 “先进先出” (FIFO) 顺序存储数据。与数组不同，不能按索引访问元素，而只能提取下一个最旧的元素。这使得它非常适合订单敏感任务，如在线订单处理或语音邮件存储。 你可以把在杂货店排队; 收银员不会选择下一个结账的人，而是会处理排队时间最长的人。

使用带有append()和pop()方法的Python列表来实现队列。这是低效的，因为当您向开始添加新元素时，列表必须按一个索引移动所有元素。

最好的做法是使用Python的collections模块中的deque类。deque对追加和弹出操作进行了优化。deque实现还允许创建双端队列，该队列可以通过popleft()和popright()方法访问队列的两端。

```python
from collections import deque
# Initializing a queue
q = deque()
# Adding elements to a queue
q.append('a')
q.append('b')
q.append('c')
print("Initial queue")
print(q)
# Removing elements from a queue
print("\nElements dequeued from the queue")
print(q.popleft())
print(q.popleft())
print(q.popleft())
print("\nQueue after removing elements")
print(q)
# Uncommenting q.popleft()
# will raise an IndexError
# as queue is now empty
```

## 3.栈(Stacks)

栈是一种连续的数据结构，充当队列的后进先出(LIFO)版本。插入到堆栈中的最后一个元素被认为是堆栈的顶部，并且是唯一可访问的元素。要访问中间元素，必须首先删除足够多的元素，使所需的元素位于堆栈顶部。

将堆栈想象成一堆餐盘;你可以把盘子加到或移到盘子堆的顶部，但必须移动整个盘子堆才能把一个放在底部。

添加元素称为push，删除元素称为pop。你可以在Python中使用内置的列表结构来实现栈。对于列表实现，推操作使用append()方法，弹出操作使用pop()。

```python
stack = []

stack.append('a')
stack.append('b')
stack.append('c')
print('Initial stack')
print(stack)

print('\nElements popped from stack:')
print(stack.pop())
print(stack.pop())
print(stack.pop())
print('\nStack after elements are popped:')
print(stack)
```

## 4.链表(Linked Lists)

链表是数据的顺序集合，使用每个数据节点上的关系指针链接到列表中的下一个节点。

与数组不同，链表在列表中没有目标位置。相反，它们基于节点串联起来。

链表中的第一个节点称为头节点，最后一个节点称为尾节点，其中尾节点的next指向为null。

链表可以是单链，也可以是双链，这取决于每个节点是只有一个指向下一个节点的指针，还是还有一个指向前一个节点的指针。

你可以把链表想象成一条链;单个链接只与相邻的链接有一个连接，但所有链接一起形成一个更大的结构。

Python没有链表的内置实现，因此需要实现一个Node类来保存数据值和一个或多个指针。

```python
class Node:
    def __init__(self, dataval=None):
        self.dataval = dataval
        self.nextval = None
class SLinkedList:
    def __init__(self):
        self.headval = None
list1 = SLinkedList()
list1.headval = Node("Mon")
e2 = Node("Tue")
e3 = Node("Wed")
# Link first Node to second node
list1.headval.nextval = e2
# Link second Node to third node
e2.nextval = e3
```