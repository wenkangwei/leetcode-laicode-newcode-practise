---
description: 储水池算法; 随机采样方法
---

# Reservoir Sampling



Reservoir Sampling 储水池算法 是用于数据量太大不清楚数据量大小的算法。它的基本思路是要保证每次随机取样中，每个样本被抽样的概率一样，如果已经知道n个数，那么每个样本的被抽样的概率保持1/n 不变

1. 在第n+1样本来的时候，P\(保留之前n个样本的可能性\) = n/\(n+1\) = P\(保留之前n个样本的 可能性 \| 已经采样的样本\)  
2. P\(前面n样本里面某个样本被替代的可能性\) = 1/n
3. P\(n+1 样本里面采样到的样本的可能性\) = 1/n \* n/\(n+1\) = 1/\(n+1\)



```text
import random
class Solution(object):
    def __init__(self):
        """
        complete the constructor if needed.
        """
        self.count = 0
        self.sample_val =0

    def read(self, value):
        """
        read a value in the stream.
        :type: value: int
        """
        self.count += 1
        
        # randomly select previous n data point
        index = random.randint(0, self.count -1)
        # the data point has 1/n possibility to be replaced by new value
        if index == 0:
          self.sample_val = value
        return self.sample_val

    def sample(self):
        """
        return the sample of already read values.
        :rtype: int
        """
        return self.sample_val
```

