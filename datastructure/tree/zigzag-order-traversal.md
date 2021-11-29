# ZigZag Order traversal

### 1. Link

{% embed url="https://www.nowcoder.com/practice/47e1687126fa461e8a3aff8632aa5559?tpId=191&rp=1&ru=%2Fta%2Fjob-code-high-algorithm&qru=%2Fta%2Fjob-code-high-algorithm%2Fquestion-ranking" %}



### 2. 描述

给定一个二叉树，返回该二叉树的之字形层序遍历，（第一层从左向右，下一层从右向左，一直这样交替）\
例如：\
给定的二叉树是{3,9,20,#,#,15,7},\
![](https://uploadfiles.nowcoder.com/images/20200807/999991351\_1596788654427\_630E55F47DBAFBF72C88E265929E43F7)\
该二叉树之字形层序遍历的结果是\


> \[\[3],\[20,9],\[15,7]]

### 示例1

输入：

```
{1,#,2}
```

复制返回值：

```
[[1],[2]]
```



### 3. 思路

1. BFS using queue to traverse the tree as usual
2. but when appending node to queue, we append them to the second queue, the queue storing the nodes in the next level
3. we also append the values of current nodes to a list
4. when queue is empty and nodes in current level are finished,  check if we need to reverse result in the current list and reverse them if necessary, then update queue with the nodes in the second queue in the next level.

### 4. Coding

```
class Solution:
    def zigzagLevelOrder(self , root ):
        # input: tree root 
        # output: traversal results
        # idea: BFS
        # 1.  traverse the tree in level-order as usual, but append the result
        #    of the same level to the second queue
        # 2. reverse the result queue if needed
        #
        # use a queue to store the node of current leve l
        # second queue to store nodes of the next level
        # loop until queue is empty
        #    pop the node from queue  
        #    add children to second queue
        #    when queue is empty, this level is finished
        #    append reverse node value in next level to res
        #    update queue
        
        if not root:
            return []
        queue = [root]
        level = []
        level_res = []
        res = []
        reverse = False
        while queue:
            node = queue.pop(0)
            if node.left:
                level.append(node.left)
            if node.right:
                level.append(node.right)
            # append result of current level
            level_res.append(node.val)
            if not queue:
                if reverse:
                    # reverse the next level
                    level_res.reverse()
                reverse = not reverse
                res.append(level_res[:])
                queue = level[:]
                level = []
                level_res = []
        return res
        
        
```



