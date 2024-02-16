# 代码随想录算法训练营第19天|654.最大二叉树|617.合并二叉树|700.二叉搜索树中的搜索|98.验证二叉搜索树

## 654.最大二叉树
[654.最大二叉树](https://leetcode-cn.com/problems/maximum-binary-tree/)

```python
from typing import List


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> TreeNode:
        if len(nums) == 1:
            return TreeNode(nums[0])
        node = TreeNode(0)
        # 找到数组中最大的值和对应的下标
        maxValue = 0
        maxValueIndex = 0
        for i in range(len(nums)):
            if nums[i] > maxValue:
                maxValue = nums[i]
                maxValueIndex = i
        node.val = maxValue
        # 最大值所在的下标左区间 构造左子树
        if maxValueIndex > 0:
            new_list = nums[:maxValueIndex]
            node.left = self.constructMaximumBinaryTree(new_list)
        # 最大值所在的下标右区间 构造右子树
        if maxValueIndex < len(nums) - 1:
            new_list = nums[maxValueIndex + 1:]
            node.right = self.constructMaximumBinaryTree(new_list)
        return node


def main():
    s = Solution()
    s1 = list(map(int, input().split()))
    printTree(s.constructMaximumBinaryTree(s1))


def printTree(root: TreeNode):
    if root is None:
        return
    print(root.val, end=" ")
    printTree(root.left)
    printTree(root.right)


main()

```

## 617.合并二叉树
[617.合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:
        # 递归终止条件:
        #  但凡有一个节点为空, 就立刻返回另外一个. 如果另外一个也为None就直接返回None.
        if not root1:
            return root2
        if not root2:
            return root1
        # 上面的递归终止条件保证了代码执行到这里root1, root2都非空.
        root1.val += root2.val  # 中
        root1.left = self.mergeTrees(root1.left, root2.left)  # 左
        root1.right = self.mergeTrees(root1.right, root2.right)  # 右

        return root1  # 本题我们重复使用了题目给出的节点而不是创建新节点. 节省时间, 空间.


def main():
    # 创建第一棵树
    #       1
    #      / \
    #     3   2
    #    /
    #   5
    root1 = TreeNode(1)
    root1.left = TreeNode(3)
    root1.right = TreeNode(2)
    root1.left.left = TreeNode(5)

    # 创建第二棵树
    #       2
    #      / \
    #     1   3
    #      \   \
    #       4   7
    root2 = TreeNode(2)
    root2.left = TreeNode(1)
    root2.right = TreeNode(3)
    root2.left.right = TreeNode(4)
    root2.right.right = TreeNode(7)

    # 创建 Solution 实例
    solution = Solution()

    # 合并两棵树
    merged_tree = solution.mergeTrees(root1, root2)

    # 打印合并后的树的前序遍历结果
    print_preorder(merged_tree)


def print_preorder(root):
    if root:
        print(root.val, end=" ")
        print_preorder(root.left)
        print_preorder(root.right)


if __name__ == "__main__":
    main()

```

## 700.二叉搜索树中的搜索
[700.二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        # 为什么要有返回值:
        #   因为搜索到目标节点就要立即return，
        #   这样才是找到节点就返回（搜索某一条边），如果不加return，就是遍历整棵树了。

        if not root or root.val == val:
            return root

        if root.val > val:
            return self.searchBST(root.left, val)

        if root.val < val:
            return self.searchBST(root.right, val)


def main():
    # 创建第一棵树
    #       4
    #      / \
    #     2   7
    #    / \
    #   1   3
    root = TreeNode(4)
    root.left = TreeNode(2)
    root.right = TreeNode(7)
    root.left.left = TreeNode(1)
    root.left.right = TreeNode(3)

    # 创建 Solution 实例
    solution = Solution()

    # 合并两棵树
    search_result = solution.searchBST(root, 2)

    # 打印合并后的树的前序遍历结果
    print_preorder(search_result)


def print_preorder(root):
    if root:
        print(root.val, end=" ")
        print_preorder(root.left)
        print_preorder(root.right)


main()

```

## 98.验证二叉搜索树
[98.验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:

    def __init__(self):
        self.vec = None

    def traversal(self, root):
        if root is None:
            return
        self.traversal(root.left)
        self.vec.append(root.val)  # 将二叉搜索树转换为有序数组
        self.traversal(root.right)

    def isValidBST(self, root):
        self.vec = []  # 清空数组
        self.traversal(root)
        for i in range(1, len(self.vec)):
            # 注意要小于等于，搜索树里不能有相同元素
            if self.vec[i] <= self.vec[i - 1]:
                return False
        return True


def main():
    # 创建第一棵树
    #       2
    #      / \
    #     1   3
    root = TreeNode(2)
    root.left = TreeNode(1)
    root.right = TreeNode(3)

    # 创建 Solution 实例
    solution = Solution()

    # 合并两棵树
    search_result = solution.isValidBST(root)

    # 打印合并后的树的前序遍历结果
    print(search_result)


main()

```