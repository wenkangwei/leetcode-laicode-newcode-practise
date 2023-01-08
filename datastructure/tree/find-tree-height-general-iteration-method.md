# Find Tree height (general iteration method)



### 1.Link

{% embed url="https://www.nowcoder.com/practice/8a2b2bf6c19b4f23a9bdb9b233eefa73?tpId=188&tqId=38323&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}







### 2. 题目描述

求给定二叉树的最大深度，最大深度是指树的根结点到最远叶子结点的最长路径上结点的数量。\
示例1

### 输入

复制

```
{1,2}
```

### 返回值

复制

```
2
```

示例2

### 输入

复制

```
{1,2,3,4,#,#,5}
```

### 返回值

复制

```
3
```



### 3. 思路

1.  bottom-up method:

    1. 每个node的input: node,    ouput: max\_depth of current node
    2. 如果node是none，return 0。否则从left， right children获取 max\_depth values, 然后返回 Max(left depth, right depth) +1

    &#x20;
2. top down method:
   1. 每个node的input: node, depth of parent   ouput: None
   2. 用global的max\_depth 存放结果
   3. 如果node是none，return, 否则 max\_depth = max(max depth,  depth + 1)

### 4.Coding

Recursion method

```
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

#
# 
# @param root TreeNode类 
# @return int整型
#
class Solution:
    def maxDepth(self , root ):
        # write code here
        #idea: bottom-up, post order 
        # input: node
        # output: max depth from current node
        # 1. base case:  if node == none, return max_depth = 0
        # 2. go to left, right subtree to obtain max depth from left, right
        # 3. return max depth of left/right +1, including current parent node
        #
        if not root:
            return 0
        l_max = self.maxDepth(root.left)
        r_max = self.maxDepth(root.right)
        return max(l_max, r_max) + 1

```



Iteration DFS method

```
class Solution:
    def maxDepth(self , root ):
        #
        # DFS iteration 写法
        # iteration method: use array to simulate stack
        # recursion 传进来的参数都 写到stack的tuple里面
        #
        #
        if not root:
            return 0
        depth = 0
        stack = [(root,0, 1 )]
        max_depth = 0
        #l_max = 0
        #r_max = 0
        while stack:
            # input to child node in one level = parent node, count of parent
            # other input parameter: depth
            node, cnt,depth = stack.pop(-1)
            if cnt == 0:
                #preorder
                stack.append((node, cnt+1, depth))
                # always go to left child first， in DFS
                if node.left:
                    # input to next level
                    stack.append((node.left, 0,depth+1))
            if cnt ==1:
                #in-order
                # when cnt =1, then we can take the result from left node
                stack.append((node, cnt+1, depth))
                if node.right:
                    # input to next level
                    stack.append((node.right, 0, depth+1))
            if cnt ==2:
                # post-order back , exit tree
                # global max method
                max_depth = max(max_depth,depth)
                
        return max_depth

```



Tree 的**iteration general**的方法/DFS + **每一层都有返回的写法**

1. **在原来的 iteration method for Tree traversal 的方法上添加以下两条**
   1. **对于每一层的node的输入，可以在 stack append (node.left, cnt,  other inputs)的tuple里面给 child node进行input的添加**
   2. **对于每个node的返回值，可以用一个result stack存放子节点的返回值**
   3. **在post-order 的地方进行返回操作， 如果current node 有right node就pop result stack的最后一个value，同理如果有left node，就pop最后一个value。 注意： 一定是先pop right再pop left，因为left的result是先比right的res 储存**
2. 简单来说， input 就是在cnt=0 或1的情况 stack.append()的tuple里面添加input。 而output就是在post-order里面从result stack里面pop值出来更新当前的node的result，再append进去返回。

```python

class Solution:
    def maxDepth(self , root ):
        #
        # DFS iteration 写法
        # iteration method: use array to simulate stack
        # recursion 传进来的参数都 写到stack的tuple里面
        #
        #
        if not root:
            return 0
        depth = 0
        stack = [(root,0, 1 )]
        max_depth = 0
        # result stack
        res_stack =[]
        #l_max = 0
        #r_max = 0
        while stack:
            # input to child node in one level = parent node, count of parent
            # other input parameter: depth
            node, cnt,depth = stack.pop(-1)
            if cnt == 0:
                #preorder
                stack.append((node, cnt+1, depth))
                # always go to left child first， in DFS
                if node.left:
                    # input to next level, the third value in tuple
                    # is input to the left child
                    stack.append((node.left, 0,depth))
            if cnt ==1:
                #in-order
                # when cnt =1, then we can take the result from left node
                stack.append((node, cnt+1, depth))
                if node.right:
                    # input to next level
                    stack.append((node.right, 0, depth))
            if cnt ==2:
                # post-order back , exit tree node
                
                # Return result to stack
                # default value =0
                r_res = 0
                l_res = 0
                if res_stack and node.right:
                    r_res = res_stack.pop(-1)
                if res_stack and node.left:
                    l_res = res_stack.pop(-1)
                # append result of this node to result stack
                depth = max(l_res, r_res) +1
                res_stack.append(depth)
                
                # global max method
                #max_depth = max(max_depth,depth)
        return res_stack[-1]
```





Iteration **BFS method**/ **Level order** method

```
class Solution:
    def maxDepth(self , root ):
        #
        # BFS iteration 写法
        # iteration method: use array to simulate queue
        # 每一层的level都+= 1
        #
        #
        if not root:
            return 0
        queue = [root] # store node in current level
        level = [] # store the node in the next level
        depth = 0
        while queue:
            node = queue.pop(0)
            if node.left:
                level.append(node.left)
            if node.right:
                level.append(node.right)
            if not queue:
                depth += 1
                queue = level
                level = []
        return depth

```



