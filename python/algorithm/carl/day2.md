# 代码随想录算法训练营第二天|977.有序数组的平方|209.长度最小的子数组|59.螺旋矩阵II

## 977.有序数组的平方

[题目链接](https://leetcode.cn/problems/squares-of-a-sorted-array/)

双指针：定义一个起始点，两个尾点，其中一个用来遍历，一个存结果，在数组范围内，判断起点平方和尾点平方大小，若起点平方大存到另一个尾点，起点后移，否则尾点平方存到另一个尾点，尾点前移。另一个尾点前移
```python
from typing import List

class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        left, right, pos = 0, len(nums) - 1, len(nums) - 1
        res = [0]*len(nums)
        while left <= right:
            if nums[left] * nums[left] > nums[right] * nums[right]:
                res[pos] = nums[left] * nums[left]
                left += 1
            else:
                res[pos] = nums[right] * nums[right]
                right -= 1
            pos -= 1
        return res

def main():
    a = Solution()
    nums = [-4, -1, 0, 3, 10]
    res = a.sortedSquares(nums)
    print(res)

if __name__ == "__main__":
    main()
```

暴力解法：遍历数组，将每个元素平方，再排序
```python
from typing import List

class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            nums[i] *= nums[i]
        nums.sort()
        return nums

def main():
    a = Solution()
    nums = [-4, -1, 0, 3, 10]
    res = a.sortedSquares(nums)
    print(res)

if __name__ == "__main__":
    main()
```

## 209.长度最小的子数组

[题目链接](https://leetcode.cn/problems/minimum-size-subarray-sum/)

相关题目：904.水果成篮 76.最小覆盖子串

滑动窗口：定义窗口最大值，定义两个指针，i控制起始位置，j控制终止位置，遍历数组求和，当窗口内的和大于等于target时，记录窗口长度，和原先的最大值比较，并减去数组i位置的值，然后将i向右移动，直到窗口内的和小于target，然后将j向右移动，直到窗口内的和大于等于target，重复上述过程，直到j到达数组末尾，要是到了末尾的结果小于数组长度，返回0

```python
from typing import List

class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        i = sum = 0
        res = len(nums) + 1
        for j in range(len(nums)):
            sum += nums[j]
            while sum >= target:
                win = j - i + 1
                res = min(res, win)
                sum -= nums[i]
                i += 1
        if res <= len(nums):
            return res
        else:
            return 0

def main():
    a = Solution()
    target = 11
    nums = [1, 1, 1, 1, 1, 1, 1, 1]
    res = a.minSubArrayLen(target, nums)
    print(res)

if __name__ == "__main__":
    main()
```

## 59.螺旋矩阵II

[题目链接](https://leetcode.cn/problems/spiral-matrix-ii/)

相关题目：54.螺旋矩阵 剑指Offer29.顺时针打印矩阵

思路：主要是如何画出矩阵以及如何正确判断矩阵边界，模拟顺时针画矩阵的过程:

填充上行从左到右

填充右列从上到下

填充下行从右到左

填充左列从下到上

```python
from typing import List

class Soultion:
    def generateMatrix(self, n: int) -> List[List[int]]:
        startx = starty = 0
        count = 1
        res = [[0] * n for _ in range(n)]
        loop = mid = n // 2
        for offset in range(1, loop + 1):
            for i in range(starty, n - offset):
                res[startx][i] = count
                count += 1
            for i in range(startx, n - offset):
                res[i][n - offset] = count
                count += 1
            for i in range(n - offset, starty, -1):
                res[n - offset][i] = count
                count += 1
            for i in range(n - offset, startx, -1):
                res[i][starty] = count
                count += 1
            startx += 1
            starty += 1
        if n % 2 == 1:
            res[mid][mid] = count
        return res

def main():
    soultion = Soultion()
    target = 4
    res = soultion.generateMatrix(target)
    print(res)
    
if __name__ == "__main__":
    main()
```

## 数组总结


