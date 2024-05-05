# 代码随想录算法训练营第25天|216.组合总和III|17.电话号码的字母组合

## 216.组合总和III

[216.组合总和III](https://leetcode-cn.com/problems/combination-sum-iii/)
```py
from typing import List


class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        result = []  # 存放结果集
        self.backtracking(n, k, 0, 1, [], result)
        return result

    def backtracking(self, targetSum, k, currentSum, startIndex, path, result):
        if currentSum > targetSum:  # 剪枝操作
            return  # 如果path的长度等于k但currentSum不等于targetSum，则直接返回
        if len(path) == k:
            if currentSum == targetSum:
                result.append(path[:])
            return
        for i in range(startIndex, 9 - (k - len(path)) + 2):  # 剪枝
            currentSum += i  # 处理
            path.append(i)  # 处理
            self.backtracking(targetSum, k, currentSum, i + 1, path, result)  # 注意i+1调整startIndex
            currentSum -= i  # 回溯
            path.pop()  # 回溯


def main():
    k = 3
    n = 7
    # 创建 Solution 类的实例
    s = Solution()
    print(s.combinationSum3(k, n))


main()

```

## 17.电话号码的字母组合

[17.电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
```py
class Solution:
    def __init__(self):
        self.letterMap = [
            "",  # 0
            "",  # 1
            "abc",  # 2
            "def",  # 3
            "ghi",  # 4
            "jkl",  # 5
            "mno",  # 6
            "pqrs",  # 7
            "tuv",  # 8
            "wxyz"  # 9
        ]
        self.result = []
        self.s = ""

    def backtracking(self, digits, index):
        if index == len(digits):
            self.result.append(self.s)
            return
        digit = int(digits[index])  # 将索引处的数字转换为整数
        letters = self.letterMap[digit]  # 获取对应的字符集
        for i in range(len(letters)):
            self.s += letters[i]  # 处理字符
            self.backtracking(digits, index + 1)  # 递归调用，注意索引加1，处理下一个数字
            self.s = self.s[:-1]  # 回溯，删除最后添加的字符

    def letterCombinations(self, digits):
        if len(digits) == 0:
            return self.result
        self.backtracking(digits, 0)
        return self.result


def main():
    digits = "23"
    # 创建 Solution 类的实例
    s = Solution()
    print(s.letterCombinations(digits))


main()

```