# 最大正方形

[https://www.nowcoder.com/practice/0058c4092cec44c2975e38223f10470e?tpId=188\&rp=1\&ru=%2Fta%2Fjob-code-high-week\&qru=%2Fta%2Fjob-code-high-week\&difficulty=\&judgeStatus=\&tags=\&title=\&sourceUrl=\&gioEnter=menu](https://www.nowcoder.com/practice/0058c4092cec44c2975e38223f10470e?tpId=188\&rp=1\&ru=%2Fta%2Fjob-code-high-week\&qru=%2Fta%2Fjob-code-high-week\&difficulty=\&judgeStatus=\&tags=\&title=\&sourceUrl=\&gioEnter=menu)





```python3
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 最大正方形
# @param matrix char字符型二维数组
# @return int整型
#
import math


class Solution:
    def detect_rectangle(self, r, c, m, dp):
        import math

        length = int(math.sqrt(dp[r - 1][c]))
        # print('length:', length, 'r, c',r,c)
        l_cnt = 0
        for i in range(length + 1):
            flag = (
                r < len(m)
                and c + i < len(m[0])
                and r - 1 > 0
                and m[r][c + i] == "1"
                and m[r - 1][c + i] == "1"
            )
            if not flag:
                # return False
                break
            l_cnt += 1
        area = l_cnt * l_cnt
        return area

    def solve(self, matrix: List[List[str]]) -> int:
        # write code here
        # [[1,0,1,0,0],
        # [1,0,1,1,1],
        # [1,1,1,1,1],
        #  [1,0,0,1,0]]
        #  问题拆解： 最大， 正方形
        #  最大： 贪心算法/ DP
        #  正方形： 怎么定义正方形， 怎么检查正方形
        #  最小正方形： 1个元素1
        #  遍历每一行怎么检查正方形：
        #  1. dp矩阵存放以当前元素为左下角的正方形最大面积
        #  2. 每次遍历到 m[r][c], 就找 dp[r-1][c]在相邻正方形最大的边长
        #  3. 然后 m[r][c] 往m[r][c+1]， m[r-1][c+1] 方向检查 最大的边长次数的看是否能形成更大的正方形
        max_square = 0
        if len(matrix) == 0:
            return 0
        rows = len(matrix)
        cols = len(matrix[0])
        dp = [[0] * cols] * rows
        for r in range(rows):
            for c in range(cols):
                if matrix[r][c] == "0":
                    dp[r][c] = 0
                else:
                    if r == 0:
                        dp[r][c] = 1
                    else:
                        area = self.detect_rectangle(r, c, matrix, dp)
                        area = max(area, 1)
                        dp[r][c] = area
                            # dp[r][c] = (int(math.sqrt(dp[r - 1][c])) + 1) ** 2
                        # else:
                            # dp[r][c] = 1
                max_square = max(max_square, dp[r][c])
            print("dp row",dp[r], 'r:', r)
        # print("dp: ", dp)
        return max_square


# dp: [
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
#     [1, 0, 1, 1, 0, 4, 1, 0, 0, 0],
# ]

[
    [1, 0, 1, 1, 1, 1, 1, 0, 0, 0],
    [1, 0, 1, 1, 1, 0, 0, 0, 0, 0],
    [1, 0, 1, 0, 1, 1, 1, 0, 0, 0],
    [1, 1, 0, 1, 1, 1, 1, 0, 1, 0],
    [1, 1, 1, 1, 1, 1, 1, 0, 0, 0],
    [1, 0, 1, 1, 1, 1, 1, 1, 1, 0],
    [1, 0, 1, 1, 1, 1, 1, 1, 1, 0],
    [1, 0, 1, 1, 1, 1, 1, 1, 1, 0],
    [1, 1, 0, 1, 1, 1, 1, 1, 0, 0],
    [1, 0, 1, 1, 0, 1, 1, 0, 0, 0],
]

```
