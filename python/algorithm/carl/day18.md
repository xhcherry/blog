# 代码随想录算法训练营第18天|513.找树左下角的值|112.路径总和|106.从中序与后序遍历序列构造二叉树

## 513.找树左下角的值
[513.找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)
```py
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


from collections import deque


class Solution:
    def findBottomLeftValue(self, root):
        if root is None:
            return 0
        queue = deque()
        queue.append(root)
        result = 0
        while queue:
            size = len(queue)
            for i in range(size):
                node = queue.popleft()
                if i == 0:
                    result = node.val
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return result


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.findBottomLeftValue(root))


main()

```

## 112.路径总和
[112.路径总和](https://leetcode-cn.com/problems/path-sum/)
```py
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def traversal(self, cur: TreeNode, count: int) -> bool:
        if not cur.left and not cur.right and count == 0:  # 遇到叶子节点，并且计数为0
            return True
        if not cur.left and not cur.right:  # 遇到叶子节点直接返回
            return False

        if cur.left:  # 左
            count -= cur.left.val
            if self.traversal(cur.left, count):  # 递归，处理节点
                return True
            count += cur.left.val  # 回溯，撤销处理结果

        if cur.right:  # 右
            count -= cur.right.val
            if self.traversal(cur.right, count):  # 递归，处理节点
                return True
            count += cur.right.val  # 回溯，撤销处理结果

        return False

    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if root is None:
            return False
        return self.traversal(root, sum - root.val)


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.hasPathSum(root, 7))


main()

```

## 113.路径总和II
[113.路径总和II](https://leetcode-cn.com/problems/path-sum-ii/)
```py
from typing import List


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def __init__(self):
        self.result = []
        self.path = []

    def traversal(self, cur, count):
        if not cur.left and not cur.right and count == 0:  # 遇到了叶子节点且找到了和为sum的路径
            self.result.append(self.path[:])
            return

        if not cur.left and not cur.right:  # 遇到叶子节点而没有找到合适的边，直接返回
            return

        if cur.left:  # 左 （空节点不遍历）
            self.path.append(cur.left.val)
            count -= cur.left.val
            self.traversal(cur.left, count)  # 递归
            count += cur.left.val  # 回溯
            self.path.pop()  # 回溯

        if cur.right:  # 右 （空节点不遍历）
            self.path.append(cur.right.val)
            count -= cur.right.val
            self.traversal(cur.right, count)  # 递归
            count += cur.right.val  # 回溯
            self.path.pop()  # 回溯

        return

    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        self.result.clear()
        self.path.clear()
        if not root:
            return self.result
        self.path.append(root.val)  # 把根节点放进路径
        self.traversal(root, sum - root.val)
        return self.result


def main():
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.left.right = TreeNode(5)
    s = Solution()
    print(s.pathSum(root, 7))


main()

```

## 106.从中序与后序遍历序列构造二叉树

[106.从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```py
from typing import List


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        # 第一步: 特殊情况讨论: 树为空. (递归终止条件)
        if not postorder:
            return None

        # 第二步: 后序遍历的最后一个就是当前的中间节点.
        root_val = postorder[-1]
        root = TreeNode(root_val)

        # 第三步: 找切割点.
        separator_idx = inorder.index(root_val)

        # 第四步: 切割inorder数组. 得到inorder数组的左,右半边.
        inorder_left = inorder[:separator_idx]
        inorder_right = inorder[separator_idx + 1:]

        # 第五步: 切割postorder数组. 得到postorder数组的左,右半边.
        # ⭐️ 重点1: 中序数组大小一定跟后序数组大小是相同的.
        postorder_left = postorder[:len(inorder_left)]
        postorder_right = postorder[len(inorder_left): len(postorder) - 1]

        # 第六步: 递归
        root.left = self.buildTree(inorder_left, postorder_left)
        root.right = self.buildTree(inorder_right, postorder_right)
        # 第七步: 返回答案
        return root


def main():
    s = Solution()
    s1 = list(map(int, input().split()))
    s2 = list(map(int, input().split()))
    res = s.buildTree(s1, s2)
    print_tree(res)


def print_tree(root):
    if root:
        print(root.val, end=' ')
        print_tree(root.left)
        print_tree(root.right)


main()

```

## 105.从前序与中序遍历序列构造二叉树

[105.从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
```py
from typing import List


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def buildTree1(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        # 第一步: 特殊情况讨论: 树为空. 或者说是递归终止条件
        if not preorder:
            return None

        # 第二步: 前序遍历的第一个就是当前的中间节点.
        root_val = preorder[0]
        root = TreeNode(root_val)

        # 第三步: 找切割点.
        separator_idx = inorder.index(root_val)

        # 第四步: 切割inorder数组. 得到inorder数组的左,右半边.
        inorder_left = inorder[:separator_idx]
        inorder_right = inorder[separator_idx + 1:]

        # 第五步: 切割preorder数组. 得到preorder数组的左,右半边.
        # ⭐️ 重点1: 中序数组大小一定跟前序数组大小是相同的.
        preorder_left = preorder[1:1 + len(inorder_left)]
        preorder_right = preorder[1 + len(inorder_left):]

        # 第六步: 递归
        root.left = self.buildTree1(preorder_left, inorder_left)
        root.right = self.buildTree1(preorder_right, inorder_right)
        # 第七步: 返回答案
        return root


def main():
    s = Solution()
    s1 = list(map(int, input().split()))
    s2 = list(map(int, input().split()))
    res = s.buildTree1(s1, s2)
    print_tree(res)


def print_tree(root):
    if root:
        print(root.val, end=' ')
        print_tree(root.left)
        print_tree(root.right)


main()

```