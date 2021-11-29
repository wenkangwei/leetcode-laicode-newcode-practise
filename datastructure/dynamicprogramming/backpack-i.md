# Backpack I

### 1. Link

{% embed url="https://www.nowcoder.com/practice/2820ea076d144b30806e72de5e5d4bbf?tpId=188&tqId=38312&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}



### 2. 题目描述

已知一个背包最多能容纳物体的体积为V现有n个物品第i个物品的体积为v\_ivi​ 第i个物品的重量为w\_iwi​求当前背包最多能装多大重量的物品示例1

### 输入

[复制](javascript:void\(0\);)

```
10,2,[[1,3],[10,4]]
```

### 返回值

[复制](javascript:void\(0\);)

```
4
```

### 说明

```
第一个物品的体积为1，重量为3，第二个物品的体积为10，重量为4。只取第二个物品可以达到最优方案，取物重量为4
```



### 3. 思路

1. &#x20;2D - dp:   dp table： row = item的id,     column =考虑第i个物品后（可以选也可以不选） 剩下volume， dp\[i]\[j] = 在考虑第i个物品 时剩下volume = j情况的的最大重量
2. 遍历 n items 和 遍历 V volume values, start from j=V and end  at j=0
3. 当j < item的volume, 那么就不用考虑第i个item，于是dp\[i]\[j] = dp\[i-1]\[j] 从上个item的最大值延伸到现在第i个item的状态
4. 如果j > item的volume,  dp\[i]\[j] = max(在第i-1个item的**volume=j的情况不加第i个item的最大值**，以及在dp\[i-1]\[j-v] +w  **上一个item的加上当前第i个item的最大值 **)
5. Time: O(n^2)  Space: O(n^2)
6. Example

![](<../../.gitbook/assets/image (3).png>)

### 4. Coding

```
class Solution:
    def knapsack(self , V , n , vw ):
        #
        #idea: 2D-DP
        # state in dp[i]: 第i个物品的weight
        # dp[0] = 第一个item weight. if v[0] <= V, dp[0]= w[0] else 0
        # dp[1] 第二个item考虑要不要加上去,  如果v[1] < V,  
        # dp[1] = max(w[1], dp[i-1])
        # 如果 v[i] + v[0] cumulative volume < V:
        # dp[1] = max(w[1], dp[i-1],w[1] + dp[i-1] )
        # update weight and cumulative volume
        #
        #由于 item的组合先后是没有顺序的，对于同一件物品
        # 对应很多种不同的剩余的Volume的情况，所以要2D dp进行组合
        #dp[i][j] 表示 在面对第 i 件物品，且背包剩余容量为 j 时所能获得的最大价值
        # 这里的最大值的剩余容量是包括第i间物品之后的剩余容量
        #     volume  1   2
        # item w
        #   w=1       1    1
        #   w=2       0   2
        #
        if n ==0:
            return 0
        dp = [[0 for i in range(V+1)]]*(n+1)
        for i in range(1,n+1):
            # 从剩余量大的一段开始堆放物品
            for j in range(V,-1,-1):
                v = vw[i-1][0]
                w = vw[i-1][1]
                if v <= j:
                    # 对比没有加上 第i件物品的最大重量和 加上第i件物品的最大重量
                    dp[i][j] = max(dp[i-1][j],dp[i-1][j-v]+w )
                else:
                    # 因为放不下第i个物品，所以当前最大值等于第i-1个物品时最大值
                    dp[i][j]= dp[i-1][j]
        return dp[n][V]
        
```









