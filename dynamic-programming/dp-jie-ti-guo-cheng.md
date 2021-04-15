# DP-解题过程

1.  Memorization\(top-down\) 方法：用recursion function来找每个状态的当前最优解
2. Tabula （bottom-up） 方法： 用array存放每个状态的当前最优解 preferred
3. 做题步骤
   1. 考虑state状态 dp\[i\]是什么，一般都是在t=i时刻或者输入大小为i时的找到的最优解
   2. 从最简单的case出发，如dp\[0\],dp\[1\]考虑和罗列符合条件的各种情况，找到dp\[0\] dp\[1\] base case的转移方程： 如何根据dp\[0\], 输入的arr\[0\], arr\[1\]得到dp\[1\]
   3. 找到array的转移方程后不断重复遍历，然后结果要么是dp\[-1\]要么是max\(dp\),取决于dp\[i\]存放的东西
   4. 如果输入的是两个str或array，就需要2D DP建立2D matrix有点像motion planning里面的matrix

