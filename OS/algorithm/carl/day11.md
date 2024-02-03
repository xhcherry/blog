# 代码随想录算法训练营第11天|20.有效的括号|1047.删除字符串中的所有相邻重复项|150.逆波兰表达式求值

## 20.有效的括号

[题目链接](https://leetcode-cn.com/problems/valid-parentheses/)

```py
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for i in s:
            if i == '(':
                stack.append(')')
            elif i == '[':
                stack.append(']')
            elif i == '{':
                stack.append('}')
            elif not stack or stack[-1] != i:
                return False
            else:
                stack.pop()
        return True if not stack else False


def main():
    st = input()
    s = Solution()
    print(s.isValid(st))


main()

```

## 1047.删除字符串中的所有相邻重复项

[题目链接](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/)

```py
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for i in s:
            if stack and stack[-1] == i:
                stack.pop()
            else:
                stack.append(i)
        return ''.join(stack)

def main():
    st = input()
    s = Solution()
    print(s.removeDuplicates(st))


main()

```

## 150.逆波兰表达式求值
```py
from operator import add, sub, mul
from typing import List


class Solution:
    op_map = {'+': add, '-': sub, '*': mul, '/': lambda x, y: int(x / y)}

    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            if token not in {'+', '-', '*', '/'}:
                stack.append(int(token))
            else:
                op2 = stack.pop()
                op1 = stack.pop()
                stack.append(self.op_map[token](op1, op2))  # 第一个出来的在运算符后面
        return stack.pop()


def main():
    s = Solution()
    nums = list(map(str, input().split()))
    print(s.evalRPN(nums))


main()

```