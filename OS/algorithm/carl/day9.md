# 代码随想录算法训练营第9天|28.实现 strStr()|459.重复的子字符串

## 28.实现 strStr()

```py
class Solution:
    def getNext(self, next, s):
        j = 0
        next[0] = 0
        for i in range(1, len(s)):
            while j > 0 and s[i] != s[j]:
                j = next[j - 1]
            if s[i] == s[j]:
                j += 1
            next[i] = j

    def strStr(self, haystack: str, needle: str) -> int:
        if len(needle) == 0:
            return 0
        next = [0] * len(needle)
        self.getNext(next, needle)
        j = 0
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]:
                j = next[j - 1]
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):
                return i - len(needle) + 1
        return -1


def main():
    s = Solution()
    hay = str(input())
    need = str(input())
    print(s.strStr(hay, need))


main()

```

## 459.重复的子字符串

```py
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        if len(s) == 0:
            return False
        nxt = [0] * len(s)
        self.getNext(nxt, s)
        if nxt[-1] != 0 and len(s) % (len(s) - nxt[-1]) == 0:
            return True
        return False

    def getNext(self, nxt, s):
        nxt[0] = 0
        j = 0
        for i in range(1, len(s)):
            while j > 0 and s[i] != s[j]:
                j = nxt[j - 1]
            if s[i] == s[j]:
                j += 1
            nxt[i] = j
        return nxt


def main():
    s = Solution()
    st = input()
    print(s.repeatedSubstringPattern(st))


main()

```

## 字符串总结|双指针回顾

[字符串](https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html)

[双指针](https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html)