# 代码随想录算法训练营第8天|344.反转字符串|541. 反转字符串II|卡码网54.替换数字|151.翻转字符串里的单词|卡码网55.右旋转字符串

## 344.反转字符串

```py
from typing import List


class Solution:
    def reverseString(self, nums: List[str]) -> None:
        n = len(nums)
        for i in range(n // 2):
            nums[i], nums[n - i - 1] = nums[n - i - 1], nums[i]


def main():
    s = Solution()
    nums = list(map(str, input().split()))
    print(s.reverseString(nums))


if __name__ == "__main__":
    main()

```

## 541. 反转字符串II

```py
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        n = len(s)
        # 转换成列表方便操作字符串中的字符
        arr = list(s)
        # 使用循环，以步长为 2 * k 遍历字符串，每次处理2k个字符
        for i in range(0, n, 2 * k):
            # arr[i:i + k] = arr[i:i + k][::-1]对于每个字符片段，取前 k 个字符的切片，并用 [::-1] 来反转这个切片
            # 然后，将反转后的结果赋值回原列表 arr 中对应的位置，从而完成前 k 个字符的反转
            arr[i:i + k] = arr[i:i + k][::-1]
        # 将最终得到的列表arr转换为字符串，并返回
        return "".join(arr)


def main():
    s = Solution()
    nums = input()
    k = int(input())
    print(s.reverseStr(nums, k))


if __name__ == "__main__":
    main()

```

## 卡码网54.替换数字

```py
class Solution:
    def replaceNumber(self, s: str) -> str:
        res = list(s)
        for i in range(len(s)):
            if res[i] in ['1', '2', '3', '4', '5', '6', '7', '8', '9']:
                res[i] = 'number'
        return ''.join(res)


def main():
    s = Solution()
    num = input()
    print(s.replaceNumber(num))


if __name__ == "__main__":
    main()

```

## 151.翻转字符串里的单词

```py
class Solution:
    def reverseWords(self, s: str) -> str:
        # 将字符串拆分为单词，即转换成列表类型
        word = s.split()
        left = 0
        right = len(word) - 1
        # 反转单词
        while left < right:
            word[left], word[right] = word[right], word[left]
            left += 1
            right -= 1
        return ' '.join(word)


def main():
    s = Solution()
    str_ = input()
    print(s.reverseWords(str_))


if __name__ == "__main__":
    main()

```

## 卡码网55.右旋转字符串

```py
class Solution:
    def reverse(self, k: int, s: str) -> str:
        return s[len(s) - k:] + s[:len(s) - k]


def main():
    s = Solution()
    k = int(input())
    str_ = input()
    print(s.reverse(k, str_))


if __name__ == "__main__":
    main()

```