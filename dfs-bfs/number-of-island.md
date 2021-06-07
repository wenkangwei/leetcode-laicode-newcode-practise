# Number of island

### 1. Link

{% embed url="https://www.nowcoder.com/practice/0c9664d1554e466aa107d899418e814e?tpId=191&&tqId=37488&rp=1&ru=/ta/job-code-high-algorithm&qru=/ta/job-code-high-algorithm/question-ranking" %}

### 2. 描述

给一个01矩阵，1代表是陆地，0代表海洋， 如果两个1相邻，那么这两个1属于同一个岛。我们只考虑上下左右为相邻。  
岛屿: 相邻陆地可以组成一个岛屿（相邻:上下左右） 判断岛屿个数。

### 示例1

输入：

```text
[[1,1,0,0,0],[0,1,0,1,1],[0,0,0,1,1],[0,0,0,0,0],[0,0,1,1,1]]
```

复制返回值：

```text
3
```

复制

### 备注：

```text
01矩阵范围<=200*200
```



### 3. 描述

1. BFS
2. iterate every element in array
3. at current element, use BFS to search all adjacent element with value =1  and mark them as -1, visited, and count the area of this island
4. then island count += 1 if the area of current node &gt;0
5. Time: O\(n\), Space: O\(\# of node in this level\)

### 4. Coding

```text
class Solution:
    def solve(self , grid ):
        # write code here
        # 1. input: 2D char array, output: number of island
        # 2. BFS
        # iterate  every node in matrix, 
        #    start from current node do BFS using a queue
        #    when queue is not empty 
        #    if node is 1 then mark the visited node as -1 until all reachable 
        #    node is found
        #    then # of island += 1
        #
        # test case: 
        #
        #
        #
        if not grid or not grid[0]:
            return 0
        cnt = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                
                area = 0
                queue = [(i,j)]
                while queue:
                    r,c = queue.pop(0)
                    if  r>=0 and r <len(grid) and c>=0 and c < len(grid[0]) and grid[r][c] == '1':
                        grid[r][c] = '-1'
                        queue.extend([(r-1, c), (r, c-1), (r+1, c), (r, c+1)])
                        area += 1
                if area >0:
                    cnt += 1
                
        return cnt
                
                
```





