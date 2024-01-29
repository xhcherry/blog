# 代码随想录算法训练营第6天|242.有效的字母异位词|349.两个数组的交集|202.快乐数|1.两数之和

## 242.有效的字母异位词

[题目链接](https://leetcode-cn.com/problems/valid-anagram/)

相关题目：383.赎金信 49.字母异位词分组 438.找到字符串中所有字母异位词

```py
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        # 定义一个数组，用于存储字符串中每个字符出现的次数
        res = [0] * 26
        # 遍历字符串，将字符串中的字符转换为ASCII码，减去'a'的ASCII码，得到字符在数组中的位置，然后将该位置的值加1
        for i in s:
            res[ord(i) - ord('a')] += 1
        # 遍历字符串，将字符串中的字符转换为ASCII码，减去'a'的ASCII码，得到字符在数组中的位置，然后将该位置的值减1
        for i in t:
            res[ord(i) - ord('a')] -= 1
        # 遍历数组，如果数组中有不为0的值，说明两个字符串不是字母异位词
        for i in range(26):
            if res[i] != 0:
                return False
        return True


def main():
    s = Solution()
    print(s.isAnagram("anagram", "nagaram"))
    print(s.isAnagram("rat", "car"))


if __name__ == '__main__':
    main()

```

## 349.两个数组的交集

[题目链接](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

相关题目：350.两个数组的交集II

```py
from typing import List


class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        # 有一个数组为空，则交集为空
        if not nums1 or not nums2:
            return []
        # 初始化哈希表
        hash = {}
        # 初始化结果数组
        res = []
        # 哈希表key为nums1数组中的数，value为1
        # 因为输出结果中的每个元素是唯一，所以对于key对应的value来说“数值是多少”就无所谓，不管元素在数组中出现多少次，把value都置为1
        for i in nums1:
            if not hash.get(i):
                hash[i] = 1
        # 遍历nums2，若nums2中有数在hash中，存放到res中，value置为0
        for i in nums2:
            if hash.get(i):
                res.append(i)
                hash[i] = 0
        return res


def main():
    s = Solution()
    nums1 = list(map(int, input().split()))
    nums2 = list(map(int, input().split()))
    print(s.intersection(nums1, nums2))


if __name__ == '__main__':
    main()

```

## 202.快乐数

[题目链接](https://leetcode-cn.com/problems/happy-number/)

```py
class Solution:
    # 求正整数 num 每个位置上数字的平方和
    def getNext(self, num):
        happy_sum = 0
        while num:
            happy_sum += (num % 10) ** 2
            num //= 10
        return happy_sum

    def isHappy(self, n: int) -> bool:
        # 定义一个集合
        mid = set()
        while True:
            # 将当前数替换为它每个位置上的数字的平方和。
            n = self.getNext(n)
            # 如果为1，则是快乐数
            if n == 1:
                return True
            # 如果替换后的数再之前出现过，则说明陷入无限循环，此数不是快乐数
            if n in mid:
                return False
            else:
                mid.add(n)


def main():
    s = Solution()
    print(s.isHappy(int(input())))


if __name__ == '__main__':
    main()

```

## 1.两数之和

[题目链接](https://leetcode-cn.com/problems/two-sum/)

```py
from typing import List


class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 定义一个字典，key为数组中的数，value为数组中数的索引
        res = dict()
        # 遍历数组
        for key, value in enumerate(nums):
            # 如果target - value在字典中，说明找到了两个数，返回两个数的索引
            if target - value in res:
                return [res[target - value], key]
            # 如果target - value不在字典中，将value存入字典中
            res[value] = key
        return []


def main():
    s = Solution()
    nums = list(map(int, input().split()))
    target = int(input())
    print(s.twoSum(nums, target))


if __name__ == "__main__":
    main()

```