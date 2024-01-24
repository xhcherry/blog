# 代码随想录算法训练营第一天|704.二分查找|27.移除元素

## 数组理论基础

[数组理论基础](https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

数组是存放在连续内存空间上的相同类型数据的集合，可以方便的通过下标索引的方式获取到下标下对应的数据；数组下标都是从0开始；数组内存空间的地址是连续的；数组的元素是不能删，只能覆盖

## 704.二分查找

[力扣链接](https://leetcode.cn/problems/binary-search)

二分法的前提条件:数组为有序数组，数组中无重复元素，因为一旦有重复元素，使用二分查找法返回的元素下标可能不是唯一的

定义区间，左闭右闭即[left, right]，或者左闭右开即[left, right)

思路：定义左右区间，取数组中间值，和target做比较，如果相等，返回下标，如果不相等，根据大小关系，缩小区间，继续查找

左闭右开区间的写法，left < right，right = mid，left = mid + 1

左闭右闭区间的写法，left <= right，right = mid - 1，left = mid + 1

```python
from typing import List
# 左闭右闭
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return -1

def main():
    solution = Solution()
    nums = [-1, 0, 3, 5, 9, 12]
    result = solution.search(nums, 5)
    print(result)

if __name__ == "__main__":
    main()
```
## 27.移除元素

[LC链接](https://leetcode-cn.com/problems/remove-element/)

暴力：在数组范围内，判断第一个数是不是要删的，若是，则从第二个开始到最后往前移动一位，否则自增判断第二个数

```python
from typing import List

class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        l, r = 0, len(nums)
        while l < r:
            if nums[l] == val:
                for j in range(l + 1, r):
                    nums[j - 1] = nums[j]
                r -= 1
                l -= 1
            l += 1
        return r

def main():
    solution = Solution()
    nums = [3, 2, 2, 3]
    val = 3
    result = solution.removeElement(nums, val)
    print(result)

if __name__ == '__main__':
    main()
```

双指针：定义俩快慢指针，快指针遍历数组，若快指针不等于要删除的值，则慢指针赋值为快指针的值，慢指针自增，否则快指针右移

```python
from typing import List

class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        fast = slow = 0
        while fast < len(nums):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow

def main():
    solution = Solution()
    nums = [3, 2, 2, 3]
    val = 3
    result = solution.removeElement(nums, val)
    print(result)

if __name__ == '__main__':
    main()
```