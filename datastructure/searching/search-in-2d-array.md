# Search in 2D array

### 1. Link

{% embed url="https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&&tqId=11154&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking" %}



### 2. 描述

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。\[  \[1,2,8,9],\
&#x20; \[2,4,9,12],\
&#x20; \[4,7,10,13],\
&#x20; \[6,8,11,15]\
]

给定 target = 7，返回 true。

给定 target = 3，返回 false。<br>

### 示例1

输入：

```
7,[[1,2,8,9],[2,4,9,12],[4,7,10,13],[6,8,11,15]]
```

复制返回值：

```
true
```

复制说明：

```
存在7，返回true
```

### 示例2

输入：

```
3,[[1,2,8,9],[2,4,9,12],[4,7,10,13],[6,8,11,15]]
```

复制返回值：

```
false
```

复制说明：

```
不存在3，返回false
```



### 3. 思路

1. search from the upper left corner, since element < current element on the left,  element > current element on the bottom
2. case 1:  if current element < target -> row += 1
3. case 2:  if current element > target -> column -= 1
4. Otherwise, return true if target is found

### 4. Coding

```
class Solution:
    # array 二维列表
    def Find(self, target, array):
        #
        #input: target int, 2D array,  output: boolean 
        # idea: binary search
        # search from upper left corner
        # since element < cur on the left, >cur on the bottom, so
        # check current element:
        #  case 1: if current clement < target ->search bottom
        # case 2: if current elemtn > target -> search left side
        if not array or not array[0] or target == None:
            return False
        
        r, c = 0, len(array[0])-1
        while c >=0 and r < len(array):
            if array[r][c] <target:
                r += 1
            elif array[r][c] >target:
                c -= 1
            else:
                return True
        return False
            
```





