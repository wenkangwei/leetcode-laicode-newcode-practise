---
description: 字节；腾讯
---

# Rotate matrix



### 题目描述

有一个NxN整数矩阵，请编写一个算法，将矩阵顺时针旋转90度。

给定一个NxN的矩阵，和矩阵的阶数N,请返回旋转后的NxN矩阵,保证N小于等于300。示例1

### 输入

[复制](javascript:void\(0\);)

```
[[1,2,3],[4,5,6],[7,8,9]],3 
```

### 返回值

[复制](javascript:void\(0\);)

```
[[7,4,1],[8,5,2],[9,6,3]]
```

想法：

1. set column, row 的边界 cs_, ce_, rs re
2. 计算它目前位置的column到cs 的距离
3. 利用这个距离和cs, ce, rs, re 查找4个对应位置的element 并swap
4. cs, ce, rs, re向内部缩小一层
5. 重复2\~4直到cs >= ce位置

```
class Solution:
    def rotateMatrix(self, mat, n):
        #
        #  [1，2，3]
        #  [4, 5, 6]
        #  [7,8, 9]
        #
        #  [7，4，1]
        #  [8, 5, 2]
        #  [9,6, 3]
        # 1.  set the start and end of row and start and end boundary of column
        # 2. loop until start of row >=  end of row
        #    compute the distance of column to start of colum
        #    top = mat[r_s][c_s+d]
        #    right = mat[c_s+d][c_e]
        #    bottom = mat[r_e][c_e - d]
        #    left = mat[r_e-d ][c_s]
        if not mat:
            return mat
        r_s, r_e = 0, n-1
        c_s, c_e = 0, n-1
        while r_s <r_e:
            for d in range(0, c_e -c_s):
                tmp = mat[r_s][c_s + d]
                # left to top
                mat[r_s][c_s + d] = mat[r_e - d][c_s] 
                #bottom to left
                mat[r_e - d][c_s]  = mat[r_e][c_e-d] 
                # right to bottom
                mat[r_e][c_e-d]  = mat[r_s +d][c_e]
                # top to right
                mat[r_s +d][c_e] = tmp

            r_s += 1
            r_e -= 1
            c_s += 1
            c_e -= 1
                
        return mat
        
            
            
```

