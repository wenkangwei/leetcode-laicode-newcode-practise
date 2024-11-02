# 刷题记录

刷题口头思路表达方式：

1. 审题拆分题目关键词：
   1. 最长/最大等等： DP,
   2. 组合数目： DFS, BFS
   3. 无重复： 用字典
   4. 有无序/找出xxx： 二分/排序/树遍历/ DFS/BFS
   5. 子序列： DP 前后依赖
   6. **只要能把问题拆解成明确的子问题， 每次遍历都是子问题的重复叠加， 就不会想其他乱七八糟的corner case 加if else.， 除非是你的子问题没有想明白**
2. **刷题方法**：
   1. 刷2次
   2. **1刷刷思路和问题拆分方法以及题感， 确保第一次刷快并覆盖大量题型**
   3. **2刷刷corner case，和记住细节**



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





## 正在做：

{% embed url="https://www.nowcoder.com/practice/6d29638c85bb4ffd80c020fe244baf11?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}



## 通过部分的题目

{% embed url="https://www.nowcoder.com/practice/0058c4092cec44c2975e38223f10470e?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}



## 已刷题目

### 0、容易卡边界题

{% embed url="https://www.nowcoder.com/practice/28eb3175488f4434a4a6207f6f484f47?tpId=188&tqId=38627&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}

###

### 1. Sorting

quick sort



merge sort



heap sort



### 2. Search

{% embed url="https://www.nowcoder.com/practice/4f470d1d3b734f8aaf2afb014185b395?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}



stack & heap

{% embed url="https://www.nowcoder.com/practice/37548e94a270412c8b9fb85643c8ccc2?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}



LinkList

{% embed url="https://www.nowcoder.com/practice/3fed228444e740c8be66232ce8b87c2f?tpId=188&tqId=38562&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}

{% embed url="https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=188&tqId=38029&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-high-week%2Fquestion-ranking&tab=answerKey" %}

{% embed url="https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu%27" %}

{% embed url="https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

Tree

树遍历

{% embed url="https://www.nowcoder.com/practice/a9d0ecbacef9410ca97463e4a5c83be7?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
待刷
{% endembed %}

{% embed url="https://www.nowcoder.com/practice/e0cc33a83afe4530bcec46eba3325116?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
已刷
{% endembed %}



树+ DP

{% embed url="https://www.nowcoder.com/practice/da785ea0f64b442488c125b441a4ba4a?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
难/待刷 只对一半
{% endembed %}





DFS/BFS&#x20;

dfs/bfs 看成树的特例(遍历顺序， 限制遍历步数，遍历顺序， 剪纸等)

{% embed url="https://www.nowcoder.com/practice/0c9664d1554e466aa107d899418e814e?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

Dynamic programming

一维DP

{% embed url="https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4?tpId=188&tqId=38622&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}

{% embed url="https://www.nowcoder.com/practice/9cf027bf54714ad889d4f30ff0ae5481?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}



{% embed url="https://www.nowcoder.com/practice/6d29638c85bb4ffd80c020fe244baf11?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/b56799ebfd684fb394bd315e89324fb4?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}
8/11 通过
{% endembed %}





2维度DP

{% embed url="https://www.nowcoder.com/practice/7d21b6be4c6b429bb92d219341c4f8bb?tpId=188&tqId=38601&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week&difficulty=&judgeStatus=&tags=/question-ranking" %}







N维度DP



Graph







