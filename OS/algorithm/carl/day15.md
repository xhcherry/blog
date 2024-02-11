# 代码随想录算法训练营第15天|层序遍历|226.翻转二叉树|101.对称二叉树

## 层序遍历

102.二叉树的层序遍历

[题目链接](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```py
import collections
from typing import Optional, List


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        queue = collections.deque([root])
        result = []
        while queue:
            level = []
            for _ in range(len(queue)):
                cur = queue.popleft()
                level.append(cur.val)
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            result.append(level)
        return result


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.levelOrder(root))


main()
```

107.二叉树的层次遍历II\
199.二叉树的右视图\
637.二叉树的层平均值\
429.N叉树的层序遍历\
515.在每个树行中找最大值\
116.填充每个节点的下一个右侧节点指针\
117.填充每个节点的下一个右侧节点指针II\
104.二叉树的最大深度\
111.二叉树的最小深度\


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

[题目链接](https://leetcode-cn.com/problems/symmetric-tree/)

```py
from typing import Optional


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        return self.compare(root.left, root.right)

    def compare(self, left, right):
        # 首先排除空节点的情况
        if left is None and right is not None:
            return False
        elif left is not None and right is None:
            return False
        elif left is None and right is None:
            return True
        # 排除了空节点，再排除数值不相同的情况
        elif left.val != right.val:
            return False
            # 此时就是：左右节点都不为空，且数值相同的情况
            # 此时才做递归，做下一层的判断
        outside = self.compare(left.left, right.right)  # 左子树：左、 右子树：右
        inside = self.compare(left.right, right.left)  # 左子树：右、 右子树：左
        isSame = outside and inside  # 左子树：中、 右子树：中 （逻辑处理）
        return isSame

def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.isSymmetric(root))

main()
```
