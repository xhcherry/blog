# 代码随想录算法训练营第15天|层序遍历|226.翻转二叉树|101.对称二叉树

## 层序遍历



## 226.翻转二叉树

[题目链接](https://leetcode-cn.com/problems/invert-binary-tree/)

```py
from typing import Optional


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print_preorder(s.invertTree(root))


def print_preorder(root):
    if root:
        print(root.val, end=" ")
        print_preorder(root.left)
        print_preorder(root.right)


main()

```

## 101.对称二叉树


