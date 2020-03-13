---
layout: post
title: 【Python数据结构4】Tree
categories:
tags: 8-数据结构与算法
keywords:
description:
order: 554
---
## 定义二叉树

这里除了定义二叉树外，还实现了以下功能：
- 打印二叉树
- 前序遍历
- 中序遍历
- 后续遍历
- 查找路径


```py
# Definition for a binary tree node.
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    # 注意，标准的二叉树遍历算法中，遇到空节点返回[],而不是[None]
    # 有些场景还是需要空节点返回[None]的，例如，使用inorder+preorder确定一棵树的场景
    def InOrder(self, root):  # LDR
        return [] if (root is None) else self.InOrder(root.left) + [root.val] + self.InOrder(root.right)

    def PreOrder(self, root):  # DLR
        return [] if (root is None) else [root.val] + self.PreOrder(root.left) + self.PreOrder(root.right)

    def PostOrder(self, root):  # LRD
        return [] if (root is None) else self.PostOrder(root.left) + self.PostOrder(root.right) + [root.val]

    def PrintTree(self, root, i=0):
        '''
        打印二叉树，凹入表示法。原理是RDL遍历，旋转90度看
        '''
        tree_str = ''
        if root.right:
            tree_str += self.PrintTree(root.right, i + 1)
        if root.val:
            tree_str += ('    ' * i + '-' * 3 + str(root.val) + '\n')
        if root.left:
            tree_str += self.PrintTree(root.left, i + 1)
        return tree_str

    def find_track(self, num, root, track_str=''):
        '''
        二叉树搜索
        :param num:
        :param root:
        :param track_str:
        :return:
        '''
        track_str = track_str + str(root.val)
        if root.val == num:
            return track_str
        if root.left is not None:
            self.find_track(num, root.left, track_str + ' ->left-> ')
        if root.right is not None:
            self.find_track(num, root.right, track_str + ' ->right-> ')

      def buildTree(self, inorder, postorder):
          """
          https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/
          中序+后序确定一棵树，前提是list中没有重复的数字
          :type inorder: List[int]
          :type postorder: List[int]
          :rtype: TreeNode
          """
          if not inorder or not postorder:
              return None
          root = TreeNode(postorder.pop())
          inorder_index = inorder.index(root.val)

          root.right = self.buildTree(inorder[inorder_index + 1:], postorder)
          root.left = self.buildTree(inorder[:inorder_index], postorder)

          return root

      def buildTree(self, preorder, inorder):
          """
          https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/
          前序+中序确定一棵树，前提是list中没有重复的数字
          pop(0)效率很低，看看怎么解决
          :type preorder: List[int]
          :type inorder: List[int]
          :rtype: TreeNode
          """
          if not preorder or not inorder:
              return None
          root=TreeNode(preorder.pop(0))
          inorder_index=inorder.index(root.val)

          root.left=self.buildTree(preorder,inorder[:inorder_index])
          root.right=self.buildTree(preorder,inorder[inorder_index+1:])
          return root

    def list2tree(self, i=1, list_num=[]):
        # 顺序结构转链式结构
        # 节点标号从1开始
        if i > len(list_num):
            return None
        treenode = TreeNode(list_num[i - 1])
        treenode.left = self.list2tree(2 * i, list_num)
        treenode.right = self.list2tree(2 * i + 1, list_num)
        return treenode

    # 还未完成
    def tree2list(self):
        pass

    def levelOrder(self, root):
        """
        https://leetcode.com/problems/binary-tree-level-order-traversal/description/
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if root is None:
            return []
        import collections
        level = 0
        deque = collections.deque([(root,0)])
        output=[]
        while deque:
            tmp_root,level = deque.popleft()
            if tmp_root.left: deque.append((tmp_root.left,level+1))
            if tmp_root.right: deque.append((tmp_root.right,level+1))
            if len(output)<=level:
                output.append([tmp_root.val])
            else:
                output[level].append(tmp_root.val)
        return output

    def levelOrder(self, root):
        """
        针对N-ary Tree的方法，binary tree 可以参考
        https://leetcode.com/problems/n-ary-tree-level-order-traversal/description/
        :type root: Node
        :rtype: List[List[int]]
        """
        q, ret = [root], []
        while any(q):
            ret.append([node.val for node in q])
            q = [child for node in q for child in node.children if child]
        return ret

    def list2tree2(self, nums):
        # 与 list2tree 的区别是：对空值不再生成子节点，之后的数据也不会作为这个空节点的子节点，而是跳过，因此更加节省空间。
        if not nums:
            return None
        nodes = [None if val is None else TreeNode(val) for val in nums]
        kids = nodes[::-1]
        root = kids.pop()
        for node in nodes:
            if node:
                if kids: node.left = kids.pop()
                if kids: node.right = kids.pop()
        return root

    def deserialize(self, string):
        # LeetCode官方版本
        # https://leetcode.com/problems/recover-binary-search-tree/discuss/32539/Tree-Deserializer-and-Visualizer-for-Python
        # deserialize('[2,1,3,0,7,9,1,2,null,1,0,null,null,8,8,null,null,null,null,7]')
        if string == '{}':
            return None
        nodes = [None if val == 'null' else TreeNode(int(val)) for val in string.strip('[]{}').split(',')]
        kids = nodes[::-1]
        root = kids.pop()
        for node in nodes:
            if node:
                if kids: node.left = kids.pop()
                if kids: node.right = kids.pop()
        return root

    def drawtree(self, root):
        # 用 turtle 画 Tree，比纯字符串美观，但慢
        def height(root):
            return 1 + max(height(root.left), height(root.right)) if root else -1

        def jumpto(x, y):
            t.penup()
            t.goto(x, y)
            t.pendown()

        def draw(node, x, y, dx):
            if node:
                t.goto(x, y)
                jumpto(x, y - 20)
                t.write(node.val, align='center', font=('Arial', 12, 'normal'))
                draw(node.left, x - dx, y - 60, dx / 2)
                jumpto(x, y - 20)
                draw(node.right, x + dx, y - 60, dx / 2)

        import turtle
        t = turtle.Turtle()
        t.speed(0);
        turtle.delay(0)
        h = height(root)
        jumpto(0, 30 * h)
        draw(root, 0, 30 * h, 40 * h)
        t.hideturtle()
        turtle.mainloop()
```


## 其它应用举例

### Recursive
"Top-down" Solution
```py
1. return specific value for null node
2. update the answer if needed                      // anwer <-- params
3. left_ans = top_down(root.left, left_params)      // left_params <-- root.val, params
4. right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params
5. return the answer if needed                      // answer <-- left_ans, right_ans
```
"Bottom-up" Solution
```py
1. return specific value for null node
2. left_ans = bottom_up(root.left)          // call function recursively for left child
3. right_ans = bottom_up(root.right)        // call function recursively for right child
4. return answers                           // answer <-- left_ans, right_ans, root.val
```

对于 “max-depth” 问题，具体解法如下：
```py
1. return if root is null
2. if root is a leaf node:
3.      answer = max(answer, depth)         // update the answer if needed
4. maximum_depth(root.left, depth + 1)      // call the function recursively for left child
5. maximum_depth(root.right, depth + 1)     // call the function recursively for right child

1. return 0 if root is null                 // return 0 for null node
2. left_depth = maximum_depth(root.left)
3. right_depth = maximum_depth(root.right)
4. return max(left_depth, right_depth) + 1  // return depth of the subtree rooted at root


class Solution:
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        return 0 if root is None else max(self.maxDepth(root.left),self.maxDepth(root.right))+1
```

### 1
https://leetcode.com/problems/populating-next-right-pointers-in-each-node/description/  
https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/  
（两个题对应同一个解法，解题思路就是用队列进行LevelOrder遍历）  

```py
class Solution:
    # @param root, a tree link node
    # @return nothing
    def connect(self, root):
        if not root:return
        import collections
        deque = collections.deque([(root, 0)])
        output = dict()
        while deque:
            tmp_root, level = deque.popleft()
            if tmp_root.left: deque.append((tmp_root.left, level + 1))
            if tmp_root.right: deque.append((tmp_root.right, level + 1))
            if level in output:
                output[level].next = tmp_root
            output[level] = tmp_root
```


### mergeTrees
```py
# merge
# Given two binary trees and imagine that when you put one of them to cover the other,
# some nodes of the two trees are overlapped while the others are not.
# merge them into a new binary tree.
# The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node.
# Otherwise, the NOT null node will be used as the node of new tree.
#
# Example :
# Input:
# 	Tree 1                     Tree 2
#           1                         2
#          / \                       / \
#         3   2                     1   3
#        /                           \   \
#       5                             4   7
# Output:
# Merged tree:
# 	     3
# 	    / \
# 	   4   5
# 	  / \   \
# 	 5   4   7

def mergeTrees(t1, t2):
    """
    :type t1: TreeNode
    :type t2: TreeNode
    :rtype: TreeNode
    """
    if (t1.val is not None) and (t2.val is not None):
        root = TreeNode(t1.val + t2.val)
        root.left = mergeTrees(t1.left, t2.left)
        root.right = mergeTrees(t1.right, t2.right)
        return root
    else:
        return t1 or t2

t1=list2tree(i=1,list_num=[1,2,3,4,None,None,5])
t2=list2tree(i=1,list_num=[2,9,None,None,3,None,None])

a=mergeTrees(t1, t2)
```


## 其它
```py
# 先子节点，后兄弟节点”的表示法
class Tree:
    def __init__(self, kids, next=None):
        self.kids = self.val = kids
        self.next = next
```
