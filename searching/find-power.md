# Find power

### 1. Link

{% embed url="https://www.nowcoder.com/practice/1a834e5e3e1a4b7ba251417554e07c00?tpId=13&tqId=11165&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&tab=answerKey" %}





### 2. 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。  
  
保证base和exponent不同时为0。不得使用库函数，同时不需要考虑大数问题，也不用考虑小数点后面0的位数。  
  
示例1

### 输入

[复制](javascript:void%280%29;)

```text
2.00000,3
```

### 返回值

[复制](javascript:void%280%29;)

```text
8.00000
```



### 3. 思路

1. base^exponent = x^n = x^\(  1\*2^1 + 1\*2^2 + .. + 1\*2^n\)= x^\(1\*2\*1\) \* x^\(1\*2\*2\)  \* ...x^\(1\*2\*n\)
2. 而其中 x^\(1\*2\*i\) 就是累积的base
3. 可以通右移 exponent 的bit 看是否需要乘上对应的这个base
4. Time: O\(logn\), Space: O\(1\) 

 

### 4. Coding

```text
# -*- coding:utf-8 -*-
class Solution:
    def Power(self, base, exponent):
        #
        # write code here
        # 
        #用于倒数
        sign = False
        if exponent <0:
            sign = True
            exponent *= -1
        res = 1
        # base^exp = base ^ (2^1 + 2^2 +.. 2^n)
        # = base^(2^1) * base^(2^2) * ... *base^(2^n)
        # 如果对于的base^(2^i) 是存在，就把那个base 乘上去 res里面
        while exponent >0:
            if exponent & 1 ==1:
                res = res * base
            exponent >>= 1
            # 计算每个base^(2^i)
            base *= base
        return 1/res if sign else res
```






