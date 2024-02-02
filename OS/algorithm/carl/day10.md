# 代码随想录算法训练营第10天|232.用栈实现队列|225.用队列实现栈

## 232.用栈实现队列

```py
class MyQueue:

    def __init__(self):
        self.stack_in = []
        self.stack_out = []

    def push(self, x: int) -> None:
        self.stack_in.append(x)

    def pop(self) -> int:
        # 如果为空
        if self.empty():
            return None
        # 如果输出栈不为空，返回输出栈中的元素
        if self.stack_out:
            return self.stack_out.pop()
        # 输出栈为空,将输入栈的元素压入输出栈
        else:
            for i in range(len(self.stack_in)):
                self.stack_out.append(self.stack_in.pop())
            return self.stack_out.pop()

    def peek(self) -> int:
        ans = self.pop()
        self.stack_out.append(ans)
        return ans

    def empty(self) -> bool:
        return not (self.stack_in or self.stack_out)


def main():
    # 创建 MyQueue 实例
    my_queue = MyQueue()

    # 测试 push 方法
    my_queue.push(1)
    my_queue.push(2)
    my_queue.push(3)

    # 测试 pop 方法
    popped_element = my_queue.pop()
    print("Popped element:", popped_element)

    # 测试 peek 方法
    peeked_element = my_queue.peek()
    print("Peeked element:", peeked_element)

    # 测试 empty 方法
    is_empty = my_queue.empty()
    print("Is queue empty?", is_empty)


if __name__ == "__main__":
    main()

```

## 225.用队列实现栈

```py
class MyStack:

    def __init__(self):
        # 初始化主队列和辅助队列
        self.major_queue = []
        self.help_queue = []

    def push(self, x: int) -> None:
        # 新元素放入辅助队列的队首
        self.help_queue.append(x)
        # 将主队列的元素加入辅助队列
        while self.major_queue:
            self.help_queue.append(self.major_queue.pop(0))
        # 现在辅助队列元素是和栈的元素排列一致，为了后续方便理解，交换两个队列的内容
        self.major_queue = self.help_queue
        self.help_queue = []

    def pop(self) -> int:
        # 判断是否为空
        if self.empty():
            return None
        # pop移除栈顶元素，相当于移除主队列的队首元素
        return self.major_queue.pop(0)

    def top(self) -> int:
        # top获取栈顶元素，即获取主队列的队首元素
        return self.major_queue[0]

    def empty(self) -> bool:
        # 只需要判断主队列是否为空
        if not self.major_queue:
            return True
        return False


def main():
    # 创建 MyQueue 实例
    my_queue = MyStack()

    # 测试 push 方法
    my_queue.push(1)
    my_queue.push(2)
    my_queue.push(3)

    # 测试 pop 方法
    popped_element = my_queue.pop()
    print("Popped element:", popped_element)

    # 测试 peek 方法
    peeked_element = my_queue.top()
    print("Peeked element:", peeked_element)

    # 测试 empty 方法
    is_empty = my_queue.empty()
    print("Is queue empty?", is_empty)


if __name__ == "__main__":
    main()

```