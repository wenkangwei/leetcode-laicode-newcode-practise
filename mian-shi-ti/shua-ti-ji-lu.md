# 刷题记录



刷题口头思路表达方式：

1. 审题拆分题目关键词：
   1. 最长/最大/最小等等： DP,
   2. 是否能编辑/有重复： 哈希表
   3. 组合数目： DFS, BFS
   4. 无重复： 用字典/哈希表
   5. 有无序/找出xxx： 二分/排序/树遍历/ DFS/BFS
   6. 子序列 (可以连续或者不连续)： DP 前后依赖
   7. 公共子串： 2维DP
   8. 公共前缀： DP
   9. **连续子序列： 公共前缀（DP）/  单调栈**
   10. **只要能把问题拆解成明确的子问题， 每次遍历都是子问题的重复叠加， 就不会想其他乱七八糟的corner case 加if else.， 除非是你的子问题没有想明白**
2. 数据结构工具
   1. 单调栈： 栈内元素单调增/减，_**用来找和大小相关的序列问**_题 ， **比当前进栈元素i刚好小（第二大） 或刚好大（第二小）的值肯定在stack\[-1]**
   2. 双指针： 对array linkedlist， 滑动窗口
   3. **哈希表**： 去重， 映射中间结果， **key 不一定是位置索引**， **可以是某种中间结果**
   4. **DP：常数(只依赖上一次结果， 上一次结果通过max, min等过滤选择)，  一维度/二维 array (要记住依赖不同位置结果)**&#x20;
   5. DFS/BFS回溯： 排列组合 / 二维或树状搜索
   6. Tree：搜索树（右边> root>左边）  平衡树，完全数，&#x20;
   7.
3. **刷题方法**：
   1. 刷2次
   2. **1刷刷思路和问题拆分方法以及题感， 确保第一次刷快并覆盖大量题型**
   3. **2刷刷corner case，和记住细节**

**解题流程**

1. 确定问题， 题目细节： 输入， 输出， 输入范围， 输出范围， corn case
2. 确定思路方向：是什么类型题目(tree, dp, dfs, bfs等)， 用什么数据结构：list，tree, heap, stack...
3. 确定自己大概解法思路 (思路清晰的话可以跳过这一步)： 可以先不沟通思路， 自己先写伪代码
4. 定义变量：&#x20;
   1. 定义变量, 说明含义, 作用 .
   2. &#x20;定义表诉对象，为了简洁，后面说思路时直接用表述对象名称说明
5. 拆分大问题成小问题（程序是遍历重复执行， 执行的每步都是最小子问题）：
   1. 遍历/递归过程中， 有什么边界问题
   2. 什么时候应该更新哪个变量/边界
   3. 怎么更新变量（有哪些子case列出来）
   4. 做不出来时， 是否变量定义有问题
   5. 什么时候终止程序/边界条件
   6. 例子：
      1. &#x20;数组最长连续上升子序列
         1. 问题拆分： 怎么定义子序列？ 什么时候更新子序列边界？ 怎么更新子序列边界？怎么保存最长位置和返回？
      2. 二叉树中和为最大的路径
         1. 问题拆分：怎么定义路径？经不经过root？路径的左右边界怎么确定？怎么保存路径并进行对比？
6. 表达思路： 每说一步， 可以举个例子说明
7. 写代码
8. 分析复杂度



注意python细节

**-用 for去初始化矩阵列表， 不要用 \[\[0]\*n]\*m , 这种方法是弱引用， 所有行都会指向同一个行！**

```python3
dp = [[0 for i in range(cols+1) ] for j in range(rows+1) ]
```

## 动态规划解题流程：

* 确定当前问题是不是可以拆成子问题， 比如输入矩阵或者数值， 把它看成i, i+1的数值看看之间是否有依赖
* DP的存储有O(1), O(n), O(n^2)
  * 题目输入如果是1个1维度数值， 一般是O(n)
  * 如果是2维数值或多个1维数值， 一般是O(n^2)
  * 如果感觉当前最优解要依赖多个或者会变动的变量时， 一般是用O(n)方法
  * 如果感觉当前最优解依赖固定1\~3 个变量时，一般用O(1)方法
  * **最最最重要是定义清楚dp数组存放的元素物理含义！如果定义错了，就会觉得差了信息，一般就是问题拆分得不够细导致的！**
  * **DP最考验初始值边界条件以及转移公式的情况有没有漏掉！**
  * **DP题目变形：**
    * **最长的xxx**&#x20;
      * 最大和，最大乘，连续子序列，蓄水池，子序列
    * 二维DP:
      * 最长公共子序列， 最长公共子字符串（改写），最大正方形，二维路径和， 二维连续路径
  * **DP 数组的索引 一般代表含义有以下**
    * **字符串/数组的位置索引:** 多个字符串/数组就有多个索引。 或者像回文串这种∟和自己对比的话就有2个位置索引， 所以是2维DP
    * **代表限制条件**： 像背包问题， 这种在变量v和为N限制条件下， 找xxx子集最大值， 那么dp的索引就是 **限制条件+ 位置信息**。 比如 dp\[i]\[j] 代表 从0到i的数组里， v数组和为j的子集最大值
    * **注意： 只要有限制条件在，我们很难找到位置连续的最优解， 这种情况只能加多一维的DP放限制条件。**

## 二叉树解题流程：

* 把二叉树路径看成arr， 但是遍历方式是树的遍历
* 注意点:
  * 一般用递归
  * &#x20;想清楚递归节点的输入， 输出，&#x20;
  * 用bottom-up还是top-down 传参，&#x20;
  * 如果题目是加了DP， 看是否要在递归外用一个变量存放额外状态数据
* 树题目变形：
  * 树的直径
  * 树+DP： 树的最大直径（经过root）， 树的最大路径（不一定经过root）
  * 多叉树(类似DFS，BFS)
  * 搜索树：左子树所有数\<root< 右子树所有数。&#x20;
  * 满二叉树：除了叶节点外，每个节点都有左右节点
  * 完全树：树的左右子树高度<=1 /完全树， 且叶节点都在左边
  * 平衡树：树的左右子树高度<=1 /完全树

## DFS/BFS树遍历的变型题



## 图

[https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196\&tqId=37158\&rp=1\&ru=%2Factivity%2Foj\&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking\&tab=answerKey](https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196\&tqId=37158\&rp=1\&ru=%2Factivity%2Foj\&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking\&tab=answerKey)

## 数组

## 正在做：

{% embed url="https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196&tqId=37158&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://www.nowcoder.com/practice/6d29638c85bb4ffd80c020fe244baf11?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196&tqId=37158&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://www.nowcoder.com/practice/fc897457408f4bbe9d3f87588f497729?tpId=196&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/abbec6a3779940aab2cc564b22d36859?tpId=191&rp=1&ru=%2Fta%2Fjob-code-high-algorithm&qru=%2Fta%2Fjob-code-high-algorithm&difficulty=&judgeStatus=&tags=583%2C593&title=&sourceUrl=&gioEnter=menu" %}

可以想想连续子数组最大乘积的变种， 结果返回连续子序列而不是最大乘积

{% embed url="https://www.nowcoder.com/practice/abbec6a3779940aab2cc564b22d36859?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=593&title=&sourceUrl=&gioEnter=menu" %}







## 通过部分的题目/难题

{% embed url="https://www.nowcoder.com/practice/0058c4092cec44c2975e38223f10470e?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/9cf027bf54714ad889d4f30ff0ae5481?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196&tqId=37158&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://leetcode.cn/problems/median-of-two-sorted-arrays/description/" %}

{% embed url="https://www.nowcoder.com/practice/a77b4f3d84bf4a7891519ffee9376df3?tpId=196&tqId=37158&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://leetcode.cn/problems/vertical-order-traversal-of-a-binary-tree/description/?envType=problem-list-v2&envId=fkOSqLYV" %}



## 已刷题目

### 0、容易卡边界题

{% embed url="https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/?envType=problem-list-v2&envId=fkOSqLYV" %}

{% embed url="https://www.nowcoder.com/practice/28eb3175488f4434a4a6207f6f484f47?tpId=188&tqId=38627&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}



{% embed url="https://leetcode.cn/problems/median-of-two-sorted-arrays/description/" %}

{% embed url="https://leetcode.cn/problems/minimum-window-substring/" %}

### 1. Sorting

quick sort

[https://app.gitbook.com/o/MQg1qdqArWYMTe4S9fPp/s/-MU4s3p9Ql9V0G1xvT9E-2910905616/datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method](../datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method.md)

merge sort

[https://app.gitbook.com/o/MQg1qdqArWYMTe4S9fPp/s/-MU4s3p9Ql9V0G1xvT9E-2910905616/datastructure/sorting/mergesort-linkedlist](../datastructure/sorting/mergesort-linkedlist.md)

heap sort

[https://app.gitbook.com/o/MQg1qdqArWYMTe4S9fPp/s/-MU4s3p9Ql9V0G1xvT9E-2910905616/datastructure/heap-and-queue/find-the-k-largest](../datastructure/heap-and-queue/find-the-k-largest.md)

{% embed url="https://leetcode.cn/problems/merge-k-sorted-lists/" %}

{% embed url="https://www.nowcoder.com/practice/fc897457408f4bbe9d3f87588f497729?tpId=196&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

### 2. Search

{% embed url="https://www.nowcoder.com/practice/4f470d1d3b734f8aaf2afb014185b395?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/?envType=problem-list-v2&envId=fkOSqLYV" %}

## 3. stack & heap /哈希表

{% embed url="https://www.nowcoder.com/practice/37548e94a270412c8b9fb85643c8ccc2?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/1624bc35a45c42c0bc17d17fa0cba788?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

{% embed url="https://leetcode.cn/problems/minimum-window-substring/" %}



## 4.单调栈

{% embed url="https://leetcode.cn/problems/largest-rectangle-in-histogram/submissions/579178750/" %}

## 5.LinkList



{% embed url="https://leetcode.cn/problems/reverse-nodes-in-k-group/submissions/578463402/" %}

{% embed url="https://www.nowcoder.com/practice/3fed228444e740c8be66232ce8b87c2f?tpId=188&tqId=38562&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}

{% embed url="https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=188&tqId=38029&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu%27" %}

{% embed url="https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

## 6.Tree

树遍历

{% embed url="https://leetcode.cn/problems/diameter-of-n-ary-tree/?envType=problem-list-v2&envId=fkOSqLYV" %}

{% embed url="https://www.nowcoder.com/practice/a9d0ecbacef9410ca97463e4a5c83be7?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
待刷
{% endembed %}

{% embed url="https://www.nowcoder.com/practice/e0cc33a83afe4530bcec46eba3325116?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
已刷
{% endembed %}

{% embed url="https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

树+ DP

{% embed url="https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
难/待刷 只对一半， 树太大容易超时
{% endembed %}

## 7. 字符串

字符串+DP:

{% embed url="https://leetcode.cn/problems/edit-distance/" %}

{% embed url="https://leetcode.cn/problems/palindrome-partitioning-ii/" %}

{% embed url="https://leetcode.cn/problems/edit-distance/solutions/188223/bian-ji-ju-chi-by-leetcode-solution/" %}

{% embed url="https://leetcode.cn/problems/edit-distance/description/" %}

{% embed url="https://leetcode.cn/problems/largest-rectangle-in-histogram/submissions/579178750/" %}







## 8.DFS/BFS / backtracking

dfs/bfs 看成树的特例(遍历顺序， 限制遍历步数，遍历顺序， 剪纸等)

{% embed url="https://www.nowcoder.com/practice/0c9664d1554e466aa107d899418e814e?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://leetcode.cn/problems/palindrome-partitioning/description/" %}

{% embed url="https://leetcode.cn/problems/palindrome-partitioning-iii/description/?envType=problem-list-v2&envId=fkOSqLYV" %}





## 9.Dynamic programming

一维DP

像背包问题 这种限制累加数大小的情况， **dp\[i] 的key 不是输入数字的顺序， 而是被限制的容量**

**DP多想想key 是输入数组的顺序i 还是 说容量。 这key的定义很重要**



求**组合数**，遍历顺序：_先遍历物品(即选项 nums\[i])再遍历背包容量(即dp\[i] 的i为容器量)_\
求**排列数**，遍历顺序：_先遍历背包容量(即dp\[i])再遍历物品_

{% embed url="https://leetcode.cn/problems/coin-change-ii/submissions/579594920/" %}

{% embed url="https://leetcode.cn/problems/combination-sum-iv/description/" %}



[https://leetcode.cn/problems/longest-increasing-subsequence/solutions/147667/zui-chang-shang-sheng-zi-xu-lie-by-leetcode-soluti/?envType=problem-list-v2\&envId=fkOSqLYV](https://leetcode.cn/problems/longest-increasing-subsequence/solutions/147667/zui-chang-shang-sheng-zi-xu-lie-by-leetcode-soluti/?envType=problem-list-v2\&envId=fkOSqLYV)

{% embed url="https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4?tpId=188&tqId=38622&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}

{% embed url="https://www.nowcoder.com/practice/9cf027bf54714ad889d4f30ff0ae5481?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://leetcode.cn/problems/palindrome-partitioning-ii/" %}

{% embed url="https://www.nowcoder.com/practice/6d29638c85bb4ffd80c020fe244baf11?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/b56799ebfd684fb394bd315e89324fb4?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
8/11 通过
{% endembed %}

{% embed url="https://www.nowcoder.com/practice/eac1c953170243338f941959146ac4bf?tpId=191&tqId=36126&rp=1&ru=/ta/job-code-high-algorithm&qru=/ta/job-code-high-algorithm&difficulty=&judgeStatus=&tags=/question-ranking" %}
DP+Set
{% endembed %}

{% embed url="https://www.nowcoder.com/practice/abbec6a3779940aab2cc564b22d36859?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=593&title=&sourceUrl=&gioEnter=menu" %}



{% embed url="https://leetcode.cn/problems/smallest-range-ii/?envType=problem-list-v2&envId=fkOSqLYV" %}



2维度DP

{% embed url="https://www.nowcoder.com/practice/7d21b6be4c6b429bb92d219341c4f8bb?tpId=188&tqId=38601&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}







N维度DP



## 10. Graph (BFS)







