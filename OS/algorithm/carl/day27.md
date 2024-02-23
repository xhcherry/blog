# 代码随想录算法训练营第27天|93.复原IP地址|78.子集|90.子集II

## 93.复原IP地址

[93.复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)
```py
from typing import List


class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        result = []
        self.backtracking(s, 0, 0, "", result)
        return result

    def backtracking(self, s, start_index, point_num, current, result):
        if point_num == 3:  # 逗点数量为3时，分隔结束
            if self.is_valid(s, start_index, len(s) - 1):  # 判断第四段子字符串是否合法
                current += s[start_index:]  # 添加最后一段子字符串
                result.append(current)
            return

        for i in range(start_index, len(s)):
            if self.is_valid(s, start_index, i):  # 判断 [start_index, i] 这个区间的子串是否合法
                sub = s[start_index:i + 1]
                self.backtracking(s, i + 1, point_num + 1, current + sub + '.', result)
            else:
                break

    def is_valid(self, s, start, end):
        if start > end:
            return False
        if s[start] == '0' and start != end:  # 0开头的数字不合法
            return False
        num = 0
        for i in range(start, end + 1):
            if not s[i].isdigit():  # 遇到非数字字符不合法
                return False
            num = num * 10 + int(s[i])
            if num > 255:  # 如果大于255了不合法
                return False
        return True


def main():
    s = "25525511135"
    solution = Solution()
    print(solution.restoreIpAddresses(s))

main()

```

## 78.子集

[78.子集](https://leetcode-cn.com/problems/subsets/)
```py
class Solution:
    def subsets(self, nums):
        result = []
        path = []
        self.backtracking(nums, 0, path, result)
        return result

    def backtracking(self, nums, startIndex, path, result):
        result.append(path[:])  # 收集子集，要放在终止添加的上面，否则会漏掉自己
        # if startIndex >= len(nums):  # 终止条件可以不加
        #     return
        for i in range(startIndex, len(nums)):
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()


def main():
    nums = [1, 2, 3]
    solution = Solution()
    print(solution.subsets(nums))


main()

```

## 90.子集II

[90.子集II](https://leetcode-cn.com/problems/subsets-ii/)
```py
class Solution:
    def subsetsWithDup(self, nums):
        result = []
        path = []
        nums.sort()  # 去重需要排序
        self.backtracking(nums, 0, path, result)
        return result

    def backtracking(self, nums, startIndex, path, result):
        result.append(path[:])  # 收集子集
        uset = set()
        for i in range(startIndex, len(nums)):
            if nums[i] in uset:
                continue
            uset.add(nums[i])
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()


def main():
    nums = [1, 2, 2]
    solution = Solution()
    print(solution.subsetsWithDup(nums))


main()

```