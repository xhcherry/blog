# 代码随想录算法训练营第24天|理论基础|77.组合

## 理论基础

[理论基础](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 77.组合
[77. 组合](https://leetcode-cn.com/problems/combinations/)

```py
from typing import List


class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []  # 存放结果集
        self.backtracking(n, k, 1, [], result)
        return result

    def backtracking(self, n, k, startIndex, path, result):
        if len(path) == k:
            result.append(path[:])
            return
        for i in range(startIndex, n - (k - len(path)) + 2):  # 优化的地方
            path.append(i)  # 处理节点
            self.backtracking(n, k, i + 1, path, result)
            path.pop()  # 回溯，撤销处理的节点


def main():
    n = 4
    k = 2
    # 创建 Solution 类的实例
    s = Solution()
    print(s.combine(n, k))


main()

```