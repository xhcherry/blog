# 代码随想录算法训练营第16天|104.二叉树的最大深度|111.二叉树的最小深度|222.完全二叉树的节点个数

## 104.二叉树的最大深度

[题目链接](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

相关题：559.n叉树的最大深度

```py
from typing import Optional


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        return self.getdepth(root)

    def getdepth(self, node):
        if not node:
            return 0
        leftheight = self.getdepth(node.left)  # 左
        rightheight = self.getdepth(node.right)  # 右
        height = 1 + max(leftheight, rightheight)  # 中
        return height


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.maxDepth(root))


main()

```

## 111.二叉树的最小深度

[题目链接](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

```py
from typing import Optional


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        return self.getDepth(root)

    def getDepth(self, node):
        if node is None:
            return 0
        leftDepth = self.getDepth(node.left)  # 左
        rightDepth = self.getDepth(node.right)  # 右

        # 当一个左子树为空，右不为空，这时并不是最低点
        if node.left is None and node.right is not None:
            return 1 + rightDepth

        # 当一个右子树为空，左不为空，这时并不是最低点
        if node.left is not None and node.right is None:
            return 1 + leftDepth

        result = 1 + min(leftDepth, rightDepth)
        return result


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.minDepth(root))


main()

```

## 222.完全二叉树的节点个数

[题目链接](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

```py
from typing import Optional


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def getNodesNum(self, cur):
        if not cur:
            return 0
        leftNum = self.getNodesNum(cur.left)  # 左
        rightNum = self.getNodesNum(cur.right)  # 右
        treeNum = leftNum + rightNum + 1  # 中
        return treeNum

    def countNodes(self, root: Optional[TreeNode]) -> int:
        return self.getNodesNum(root)


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.countNodes(root))


main()

```