# NC91 最长上升子序列

```python3
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# retrun the longest increasing subsequence
# @param arr int整型一维数组 the array
# @return int整型一维数组
#
class Solution:
    def LIS(self , arr: List[int]) -> List[int]:
        # write code here
        # DP：
        # P(i,j)=第i到第j的substring的最优解
        # P(i，j)到P(i, j+1)的转移关系为
        # P(i, j+1) = max(P(i,i)~P(i,j)中数值中符合小于第j+1的数值条件的最优解) +1 
        # 否则如果没有符合条件的话, 即前面没有小于j+1数值的数， P(i, j+1) = 1
        # 原因： P(i, j+1)当前最优解是依赖前面P(i,i)~P(i,j) 这些最优解中符合条件的最优解
        dp = [1]*len(arr)
        max_val_idx = 0
        max_val = 1
        for j in range(1, len(arr)):
            for i in range(0, j):
                if arr[j] > arr[i]:
                    dp[j] = max(dp[i]+1, dp[j])
            if max_val < dp[j]:
                max_val = dp[j]
                max_val_idx = j
        print(dp)
        z=0 
        cur_dp = 1
        res =[]
        while z <= max_val_idx:
            if dp[z] > cur_dp:
                res.append(arr[z])
                cur_dp = dp[z]
                pass
            elif dp[z] == cur_dp:
                if len(res)==0:
                    res.append(arr[z])
                else:
                    if res[-1] > arr[z]:
                        res[-1] = arr[z]
                pass
            z+=1

        return res




```

{% embed url="https://www.nowcoder.com/practice/9cf027bf54714ad889d4f30ff0ae5481?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
