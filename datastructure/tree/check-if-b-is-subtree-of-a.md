# Check if B is Subtree of A



### 1. Link

{% embed url="https://www.nowcoder.com/practice/6e196c44c7004d15b1610b9afca8bd88?tpId=13&tqId=11170&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&tab=answerKey" %}





### 2. 题目描述

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）示例1

### 输入

复制

```
{8,8,#,9,#,2,#,5},{8,9,#,2}
```

### 返回值

复制

```
true
```



### 3. 思路

1. 定义一个对比tree A有没有包含tree B的函数
   1. 输入： tree A的某个node， treeB的root node
   2. 如果treeA的树搜索完但B的没有搜索完，就是False
   3. 如果treeB搜索完，就return True （treeB 是treeA的一部分）
   4. 否则对比当前treeA的node 和B的node的value，不一致就return False
2. 在HasSubtree 里面先对比tree A当前的node和tree B的root node，如果一致，就对比以tree A当前的node为root node的subtree和tree B
3. 如果不一致，就对比tree A当前的node的left，right分支有没有包含tree B。
4. Time: O(m) for matching subtree A and tree B, m= number of node in B,  O(n) for iterating every node in A. In total: O(m\*n)
5. Space: O(logn) for recursion

### 4. Coding

```
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def HasSubtree(self, pRoot1, pRoot2):
        # write code here
        res = False
        if pRoot1 and pRoot2:
            if pRoot1.val == pRoot2.val:
                # start from current node1 and root of B to check 
                res = self.sametree(pRoot1, pRoot2)
            if not res:
                # if doesn't match, start from left subtree, or right
                # subtree to match subtree of A and tree B
                res = self.HasSubtree(pRoot1.left, pRoot2) or self.HasSubtree(pRoot1.right, pRoot2)
        return res
    def sametree(self, node1, node2):
        if (node2 and not node1):
            # if node2 still exist, but node1 doesn't exist
            # then node2 is not subtree of node1
            return False
        if not node2:
            #if node2, B, already checked entirely, but node1 A still
            # exist, then B is subtree, part of A
            return True
        if node1.val != node2.val:
            return False
        f1= self.sametree(node1.left, node2.left) and self.sametree(node1.right, node2.right)
        return f1
```
