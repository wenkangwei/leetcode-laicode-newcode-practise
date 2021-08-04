---
description: Medium; Tree
---

# 打印Tree的右视图

### 1. Link

{% embed url="https://www.nowcoder.com/practice/c9480213597e45f4807880c763ddd5f0?tpId=188&tqId=38315&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}



### 2. 题目描述

请根据二叉树的前序遍历，中序遍历恢复二叉树，并打印出二叉树的右视图示例1

### 输入

[复制](javascript:void%280%29;)

```text
[1,2,4,5,3],[4,2,5,1,3]
```

### 返回值

[复制](javascript:void%280%29;)

```text
[1,3,5]
```



### 3. 思路

1.  先根据 preorder, inorder 进行reconstruct tree. Time: O\(nlogn\)  if tree is balance otherwise O\(n^2\)
2. level order print right view of tree.  Time: O\(n\)

### 4. Coding

```text
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
# 求二叉树的右视图
# @param xianxu int整型一维数组 先序遍历
# @param zhongxu int整型一维数组 中序遍历
# @return int整型一维数组
#
class Solution:
    def solve(self , xianxu , zhongxu ):
        # write code here
        # input: preorder, inorder
        # output: right view of tree
        # 1. reconstruct tree
        # 2.level order  to print the right view
        #
        #
        # test case: 
        #
        #
        if len(xianxu) != len(zhongxu):
            return None
        root = self.reconstruct( xianxu, zhongxu, 0, len(xianxu)-1)
        # find right view
        return self.rightview( root)
        
    def reconstruct(self, preorder, inorder, start, end ):
        # input:  preorder, inorder, start of searching region, end of searching region
        # output: root node of subtree
        #
        # Time: O(nlogn) if tree is balance otherwise O(n^2)
        # base case: when prorder list already finish or start >end is invalid
        if len(preorder)==0 or start >end :
            return None
        parent = TreeNode(preorder.pop(0))
        idx = -1
        for i in range(start, end+1):
            if inorder[i] == parent.val:
                idx = i
                break
        left = self.reconstruct( preorder, inorder, start, idx-1 )
        right = self.reconstruct( preorder, inorder, idx+1, end )
        parent.left = left
        parent.right = right
        return parent
    def rightview(self, root):
        #
        # level order traversal using queue:
        # 
        if not root:
            return []
        queue = [root]
        res = []
        level = [] # store node of the next level
        res.append(root.val)
        while queue:
            node = queue.pop(0)
            if node.left:
                level.append(node.left)
            if node.right:
                level.append(node.right)
            if not queue:
                if len(level)>0:
                    res.append(level[-1].val)
                queue = level[:]
                level = []
        return res

        
        
        
        
        
        
        
        
        
```









