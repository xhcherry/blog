# 代码随想录算法训练营第26天|39.组合总和|40.组合总和II|131.分割回文串

## 39.组合总和

[39.组合总和](https://leetcode-cn.com/problems/combination-sum/)
```py
class Solution:

    def backtracking(self, candidates, target, total, startIndex, path, result):
        if total == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            if total + candidates[i] > target:
                continue
            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i, path, result)
            total -= candidates[i]
            path.pop()

    def combinationSum(self, candidates, target):
        result = []
        candidates.sort()  # 需要排序
        self.backtracking(candidates, target, 0, 0, [], result)
        return result


def main():
    candidates = [2, 3, 6, 7]
    target = 7
    # 创建 Solution 类的实例
    s = Solution()
    print(s.combinationSum(candidates, target))


main()

```

## 40.组合总和II

[40.组合总和II](https://leetcode-cn.com/problems/combination-sum-ii/)
```py
class Solution:

    def backtracking(self, candidates, target, total, startIndex, path, result):
        if total == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            if i > startIndex and candidates[i] == candidates[i - 1]:
                continue

            if total + candidates[i] > target:
                break

            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i + 1, path, result)
            total -= candidates[i]
            path.pop()

    def combinationSum2(self, candidates, target):
        result = []
        candidates.sort()
        self.backtracking(candidates, target, 0, 0, [], result)
        return result


def main():
    candidates = [10, 1, 2, 7, 6, 1, 5]
    target = 8
    # 创建 Solution 类的实例
    s = Solution()
    print(s.combinationSum2(candidates, target))


main()

```

## 131.分割回文串

[131.分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)
```py
from typing import List


class Solution:

    def partition(self, s: str) -> List[List[str]]:
        result = []
        self.backtracking(s, 0, [], result)
        return result

    def backtracking(self, s, start_index, path, result):
        # Base Case
        if start_index == len(s):
            result.append(path[:])
            return

        # 单层递归逻辑
        for i in range(start_index, len(s)):
            # 若反序和正序相同，意味着这是回文串
            if s[start_index: i + 1] == s[start_index: i + 1][::-1]:
                path.append(s[start_index:i + 1])
                self.backtracking(s, i + 1, path, result)  # 递归纵向遍历：从下一处进行切割，判断其余是否仍为回文串
                path.pop()  # 回溯


def main():
    s = "aab"
    # 创建 Solution 类的实例
    solution = Solution()
    print(solution.partition(s))


main()

```