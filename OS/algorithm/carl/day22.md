# 代码随想录算法训练营第22天|235.二叉搜索树的最近公共祖先|701.二叉搜索树中的插入操作|450.删除二叉搜索树中的节点

## 235.二叉搜索树的最近公共祖先

[235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def traversal(self, cur, p, q):
        if cur is None:
            return cur
            # 中
        if cur.val > p.val and cur.val > q.val:  # 左
            left = self.traversal(cur.left, p, q)
            if left is not None:
                return left

        if cur.val < p.val and cur.val < q.val:  # 右
            right = self.traversal(cur.right, p, q)
            if right is not None:
                return right

        return cur

    def lowestCommonAncestor(self, root, p, q):
        return self.traversal(root, p, q)


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

## 701.二叉搜索树中的插入操作

[701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def __init__(self):
        self.parent = None

    def traversal(self, cur, val):
        if cur is None:
            node = TreeNode(val)
            if val > self.parent.val:
                self.parent.right = node
            else:
                self.parent.left = node
            return

        self.parent = cur
        if cur.val > val:
            self.traversal(cur.left, val)
        if cur.val < val:
            self.traversal(cur.right, val)

    def insertIntoBST(self, root, val):
        self.parent = TreeNode(0)
        if root is None:
            return TreeNode(val)
        self.traversal(root, val)
        return root


def main():
    root = TreeNode(4)
    root.left = TreeNode(2)
    root.right = TreeNode(7)
    root.left.left = TreeNode(1)
    root.left.right = TreeNode(3)

    # 创建 Solution 实例
    s = Solution()
    val = 5
    print(s.insertIntoBST(root, val).right.left.val)


main()

```

## 450.删除二叉搜索树中的节点

[450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def deleteNode(self, root, key):
        if root is None:
            return root
        if root.val == key:
            if root.left is None and root.right is None:
                return None
            elif root.left is None:
                return root.right
            elif root.right is None:
                return root.left
            else:
                cur = root.right
                while cur.left is not None:
                    cur = cur.left
                cur.left = root.left
                return root.right
        if root.val > key:
            root.left = self.deleteNode(root.left, key)
        if root.val < key:
            root.right = self.deleteNode(root.right, key)
        return root


def main():
    root = TreeNode(5)
    root.left = TreeNode(3)
    root.right = TreeNode(6)
    root.left.left = TreeNode(2)
    root.left.right = TreeNode(4)
    root.right.right = TreeNode(7)

    # 创建 Solution 实例
    s = Solution()
    key = 3
    print(s.deleteNode(root, key).val)


main()

```