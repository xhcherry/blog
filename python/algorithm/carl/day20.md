# 代码随想录算法训练营第20天|530.二叉搜索树的最小绝对差|501.二叉搜索树中的众数|236. 二叉树的最近公共祖先

## 530.二叉搜索树的最小绝对差
[530.二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)
```python
from typing import Optional


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:

    def __init__(self):
        self.vec = []

    def traversal(self, root):
        if root is None:
            return
        self.traversal(root.left)
        self.vec.append(root.val)  # 将二叉搜索树转换为有序数组
        self.traversal(root.right)

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.vec = []
        self.traversal(root)
        if len(self.vec) < 2:
            return 0
        result = float('inf')
        for i in range(1, len(self.vec)):
            # 统计有序数组的最小差值
            result = min(result, self.vec[i] - self.vec[i - 1])
        return result


def main():
    # 创建第二棵树
    #       4
    #      / \
    #     2   6
    #    /  \
    #   1   3
    root2 = TreeNode(4)
    root2.left = TreeNode(2)
    root2.right = TreeNode(6)
    root2.left.left = TreeNode(1)
    root2.left.right = TreeNode(3)

    # 创建 Solution 实例
    s = Solution()
    print(s.getMinimumDifference(root2))


main()

```

## 501.二叉搜索树中的众数
[501.二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)
```python
from typing import Optional, List
from collections import defaultdict


class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def searchBST(self, cur, freq_map):
        if cur is None:
            return
        freq_map[cur.val] += 1  # 统计元素频率
        self.searchBST(cur.left, freq_map)
        self.searchBST(cur.right, freq_map)

    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        freq_map = defaultdict(int)  # key:元素，value:出现频率
        result = []
        if root is None:
            return result
        self.searchBST(root, freq_map)
        max_freq = max(freq_map.values())
        for key, freq in freq_map.items():
            if freq == max_freq:
                result.append(key)
        return result


def main():
    #      1
    #       \
    #       2
    #      /
    #     2
    root2 = TreeNode(1)
    root2.right = TreeNode(2)
    root2.right.left = TreeNode(2)

    # 创建 Solution 实例
    s = Solution()
    print(s.findMode(root2))


main()

```

## 236. 二叉树的最近公共祖先
[236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root == q or root == p or root is None:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left is not None and right is not None:
            return root

        if left is None and right is not None:
            return right
        elif left is not None and right is None:
            return left
        else:
            return None


def main():
    root = TreeNode(4)
    root.left = TreeNode(2)
    root.right = TreeNode(7)
    root.left.left = TreeNode(1)
    root.left.right = TreeNode(3)

    # 创建 Solution 实例
    s = Solution()
    p = root.left
    q = root.right
    print(s.lowestCommonAncestor(root, p, q).val)


main()

```