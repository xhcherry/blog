# 代码随想录算法训练营第23天|669. 修剪二叉搜索树|108.将有序数组转换为二叉搜索树|538.把二叉搜索树转换为累加树

## 669.修剪二叉搜索树
[669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def trimBST(self, root: TreeNode, low: int, high: int) -> TreeNode:
        if root is None:
            return None
        if root.val < low:
            # 寻找符合区间 [low, high] 的节点
            return self.trimBST(root.right, low, high)
        if root.val > high:
            # 寻找符合区间 [low, high] 的节点
            return self.trimBST(root.left, low, high)
        root.left = self.trimBST(root.left, low, high)  # root.left 接入符合条件的左孩子
        root.right = self.trimBST(root.right, low, high)  # root.right 接入符合条件的右孩子
        return root


def main():
    root = TreeNode(3)
    root.left = TreeNode(0)
    root.right = TreeNode(4)
    root.left.right = TreeNode(2)
    root.left.right.left = TreeNode(1)

    # 创建 Solution 实例
    s = Solution()
    low = 1
    high = 3
    print(s.trimBST(root, low, high).val)


main()

```

## 108.将有序数组转换为二叉搜索树
[108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
```python
from typing import List


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def traversal(self, nums: List[int], left: int, right: int) -> TreeNode:
        if left > right:
            return None

        mid = left + (right - left) // 2
        root = TreeNode(nums[mid])
        root.left = self.traversal(nums, left, mid - 1)
        root.right = self.traversal(nums, mid + 1, right)
        return root

    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        root = self.traversal(nums, 0, len(nums) - 1)
        return root


def main():
    nums = [-10, -3, 0, 5, 9]

    # 创建 Solution 实例
    s = Solution()
    print(s.sortedArrayToBST(nums).val)


main()

```

## 538.把二叉搜索树转换为累加树
[538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.pre = 0  # 记录前一个节点的数值
        self.traversal(root)
        return root

    def traversal(self, cur):
        if cur is None:
            return
        self.traversal(cur.right)
        cur.val += self.pre
        self.pre = cur.val
        self.traversal(cur.left)


def main():
    root = TreeNode(4)
    root.left = TreeNode(1)
    root.right = TreeNode(6)
    root.left.left = TreeNode(0)
    root.left.right = TreeNode(2)
    root.right.left = TreeNode(5)
    root.right.right = TreeNode(7)

    # 创建 Solution 实例
    s = Solution()
    print(s.convertBST(root).val)


main()

```
