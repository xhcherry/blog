# 代码随想录算法训练营第7天|454.四数相加II|383.赎金信|15.三数之和|18.四数之和

## 454.四数相加II

[题目链接](https://leetcode-cn.com/problems/4sum-ii/)

```py
from typing import List


class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        # 使用字典存储nums1和nums2中的元素及其和
        hashmap = dict()
        # 遍历nums1和nums2中的元素，计算其和并存入字典
        for n1 in nums1:
            for n2 in nums2:
                if n1 + n2 in hashmap:
                    hashmap[n1 + n2] += 1
                else:
                    hashmap[n1 + n2] = 1
        count = 0
        # 遍历nums3和nums4中的元素，计算其和的相反数是否在字典中，若在则将其对应的value值加入count中
        for n3 in nums3:
            for n4 in nums4:
                key = -n3 - n4
                if key in hashmap:
                    count += hashmap[key]
        return count


def main():
    s = Solution()
    nums1 = list(map(int, input().split()))
    nums2 = list(map(int, input().split()))
    nums3 = list(map(int, input().split()))
    nums4 = list(map(int, input().split()))
    print(s.fourSumCount(nums1, nums2, nums3, nums4))


if __name__ == '__main__':
    main()
```

## 383.赎金信

[题目链接](https://leetcode-cn.com/problems/ransom-note/)

```py
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # 使用两个数组存储ransomNote和magazine中各个字母出现的次数
        ransom_count = [0] * 26
        magazine_count = [0] * 26
        # 遍历ransomNote和magazine，统计各个字母出现的次数
        for i in ransomNote:
            ransom_count[ord(i) - ord('a')] += 1
        for i in magazine:
            magazine_count[ord(i) - ord('a')] += 1
        # 判断ransomNote中的字母出现次数是否小于等于magazine中的字母出现次数
        return all(ransom_count[i] <= magazine_count[i] for i in range(26))


def main():
    a = input()
    b = input()
    s = Solution()
    print(s.canConstruct(a, b))


if __name__ == "__main__":
    main()

```

## 15.三数之和

[题目链接](https://leetcode-cn.com/problems/3sum/)

```py
from typing import List


class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # 用于存储结果
        result = []
        nums.sort()
        # 遍历nums
        for i in range(len(nums)):
            # 如果第一个元素已经大于0，不需要进一步检查
            if nums[i] > 0:
                return result
            # 跳过相同的元素以避免重复
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            # 双指针
            left = i + 1
            right = len(nums) - 1
            # 双指针移动
            while right > left:
                # 计算三个数的和
                sum_ = nums[i] + nums[left] + nums[right]
                if sum_ < 0:
                    left += 1
                elif sum_ > 0:
                    right -= 1
                else:
                    # 如果三个数的和为0，将其加入到结果中
                    result.append([nums[i], nums[left], nums[right]])
                    # 跳过相同的元素以避免重复
                    while right > left and nums[right] == nums[right - 1]:
                        right -= 1
                    while right > left and nums[left] == nums[left + 1]:
                        left += 1
                    right -= 1
                    left += 1
        return result


def main():
    s = Solution()
    num = list(map(int, input().split()))
    print(s.threeSum(num))


if __name__ == "__main__":
    main()
```

## 18.四数之和

[题目链接](https://leetcode-cn.com/problems/4sum/)

```py
from typing import List


class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        result = []
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            for j in range(i + 1, len(nums)):
                if j > i + 1 and nums[j] == nums[j - 1]:
                    continue
                left = j + 1
                right = len(nums) - 1
                while left < right:
                    sum_ = nums[i] + nums[j] + nums[left] + nums[right]
                    if sum_ < target:
                        left += 1
                    elif sum_ > target:
                        right -= 1
                    else:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left + 1]:
                            left += 1
                        while left < right and nums[right] == nums[right - 1]:
                            right -= 1
                        left += 1
                        right -= 1
        return result


def main():
    s = Solution()
    num = list(map(int, input().split()))
    target = int(input())
    print(s.fourSum(num, target))


if __name__ == "__main__":
    main()

```