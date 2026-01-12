# 刷题记录- 202512

## 已做题目

#### **sorting**

* quick sort/ quick selection

{% content-ref url="../../datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method.md" %}
[find-topk-largest-quickselect-kuai-su-xuan-ze-method.md](../../datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method.md)
{% endcontent-ref %}

* 思路： divide and conquer. 每次找到start, end-1 范围内array\[end]适合插入的位置，该位置左边是大于array\[end]的所以元素， 右边是所有小于array\[end]的元素



#### **searching**

{% embed url="https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array-ii/description/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路: 旋转数组考虑2点： arr\[left]和arr\[right]大小关系 和arr\[mid]与arr\[left]之间关系，怎么在非递增矩阵情况下right边界一直默认左移且默认在最小的结果位置上而不会死循环。



{% embed url="https://www.nowcoder.com/practice/4f470d1d3b734f8aaf2afb014185b395?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

*   思路：移动边界时，+1， -1是为了避免死循环，不+/-1是为了保留数值

    ```python3
    class Solution:
        def search(self , nums: List[int], target: int) -> int:
            if not nums:
                return -1
            # write code here
            l, r = 0, len(nums)
            while l<r:
                mid = (l+r)//2
                if nums[mid] <target:
                    # +1是确保在 r= l+1时 不会mid=l 导致死循环
                    l = mid+1
                elif nums[mid] > target:
                    # -1 是确保在 r= l+1时 不会mid=r 导致死循环
                    r = mid-1
                else:
                    #确保找到target后，l和r至少有一个是包含target
                    r = mid
            if l< len(nums) and nums[l]==target:
                return l
            if r >=0 and nums[r] == target:
                return r
            return -1
    ```



#### LinkedList

{% embed url="https://leetcode.cn/problems/swap-nodes-in-pairs/description/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路： fast slow pointer， 对 slow.next节点开始, fast 节点结尾的group进行反转

{% embed url="https://leetcode.cn/problems/reverse-nodes-in-k-group/submissions/690537596/" %}

{% embed url="https://leetcode.cn/problems/partition-list/description/?envType=problem-list-v2&envId=fkOSqLYV" %}



{% embed url="https://leetcode.cn/problems/add-two-numbers-ii/submissions/690905297/" %}



#### Two pointer

{% embed url="https://leetcode.cn/problems/container-with-most-water/?envType=problem-list-v2&envId=fkOSqLYV" %}



#### **heap/Array/Stack**

{% embed url="https://leetcode.cn/problems/merge-k-sorted-lists/" %}

* 思路: min-heap, 时间复杂度nlogk

{% embed url="https://www.nowcoder.com/practice/1624bc35a45c42c0bc17d17fa0cba788?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

* 滑动窗口+ 单调队列（单调递减队列存放window里第1，2，3.。k大值）



{% embed url="https://leetcode.cn/problems/0ynMMM/submissions/689820599/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路： 单调栈 存储， left\[i] = 从左往右数比heights\[i] 小的最大数 right\[i]同理

#### Set

{% embed url="https://leetcode.cn/problems/longest-substring-without-repeating-characters/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路: sliding window + set

#### **DFS**

**permutation 排列**

{% embed url="https://leetcode.cn/problems/permutations-ii/" %}

{% embed url="https://leetcode.cn/problems/permutations/submissions/688951631/" %}

* 思路： dfs + 排序去重逻辑
  * 要先把选项排序，然后在dfs的递归里的for循环中对比 opt\[i-1], opt\[i]把已经选择过的元素skip掉



**combination**

{% embed url="https://leetcode.cn/problems/combination-sum-ii/description/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路:  类似凑硬币问题，有2个问题先要解决
  * 选项数字可能重复，需要先通过字典进行去重和次数存储
    * dfs recursion里面的**for循环 是当前选项数字的使用次数**。(**排序问题里面的for循环是当前位置选择用哪个数字**)&#x20;
  * terminal state 递归层数最大是去重后的选项数 或者 和为target的组合数字的层数。有2个条件。



{% embed url="https://leetcode.cn/problems/combination-sum-iii/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路：组合问题，不是排列问题， recursion 里只有2个child/ 选和不选。 重点是终止条件的先后
* 备注： 细节问题， 注意传参被传错



**Palidrome回文数**

{% embed url="https://leetcode.cn/problems/palindrome-partitioning/description/" %}

* 思路:  分割回文数得到所有**回文数组合**， 和 **全排列逻辑类似**， 区别只在
  * for循环里面从选择数字变成 选择 边界数字(分割的最后一个数字)
  * for循环里面，进入下一层递归时，**从起始位置pos +1变成终止位置i+1 来跳过中间的分割选项**
  * **加入dp存储 回文数状态**，dp\[i]\[j] 取决于dp\[i+1]\[j-1] 和s\[i]==s\[j]
* 回文数判断方法:
  * 分奇数偶数长度情况判断corner case， 然后从外到内或内到外延申判断



#### **BFS**

{% embed url="https://www.nowcoder.com/practice/0c9664d1554e466aa107d899418e814e?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

* 思路： bfs， 遍历整个matrix， 每次找到element=1 就bfs queue。 注意：queue append要是当前位置的上下左右4个方向，而不是右下，因为queue append的方向并不是往右下角遍历



#### **Tree**

树遍历

{% embed url="https://leetcode.cn/problems/6eUYwP/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路： **前缀和 + 树遍历，  可以参考和为target的子数组题目，只是变成树结构遍历而**已

{% embed url="https://www.nowcoder.com/practice/e0cc33a83afe4530bcec46eba3325116?tpId=188&tags=583&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

* 注意：o1, o2不能是ancester
* 代码

```python3
class Solution:
    def helper(self, root, o1, o2):
        if not root:
            return -1

        l = self.helper(root.left, o1, o2)
        r = self.helper(root.right, o1, o2)
        if (l and r ) or (root.val in [o1, o2] and (l or r)):
            self.ancester = root.val
            
        if root.val in [o1,o2] or l or r:
            return 1
        return -1

    def helper2(self, root, o1, o2):
        if not root:
            return None
        if root.val in [o1, o2]:
            return root
        t1 = self.helper2(root.left, o1,o2)
        t2 = self.helper2(root.right, o1,o2)
        if not t1:
            return t2
        if not t2:
            return t1
        return root


    def lowestCommonAncestor(self , root: TreeNode, o1: int, o2: int) -> int:
        # write code here
        # self.ancester = -1
        # self.helper(root, o1,o2)
        # return self.ancester
        val= self.helper2(root,o1,o2)
        if val:
            return val.val
        return -1
```

{% embed url="https://www.nowcoder.com/practice/a9d0ecbacef9410ca97463e4a5c83be7?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

* recursion swap 左右节点即可

{% embed url="https://leetcode.cn/problems/sum-root-to-leaf-numbers/submissions/690915446/?envType=problem-list-v2&envId=fkOSqLYV" %}



#### Zig-zag Order / BFS

{% embed url="https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/?envType=problem-list-v2&envId=fkOSqLYV" %}





#### DP

* dp思路技巧回顾
  * 问题凡是带 "最xx" 最长最短等等 找某个状态的极端值，用dp存储状态 + 贪心算法找极值
  * dp存储的状态1个维度是存答案的状态，其他维度是其他中间状态。&#x20;
    * 比如 ”最长长度“，dp\[i]\[j] 里i存长度， j可以存其他维度状态



{% embed url="https://leetcode.cn/problems/maximum-product-subarray/?envType=problem-list-v2&envId=fkOSqLYV" %}

```
// Some code 最大累加和思路

def max_sum(nums):
    max_sum = -float('inf')
    res = max_sum
    for i in range(len(nums)):
        max_sum = max(max_sum, 0) + nums[i]
        res = max(res, max_sum)
    return res
```







{% embed url="https://leetcode.cn/problems/longest-arithmetic-subsequence/submissions/689810933/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路：二维dp， dp\[i]\[d], i是以第i个数字结尾的等差=d的子序列长度

{% embed url="https://leetcode.cn/problems/edit-distance/submissions/689332682/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路: 二维dp，dp\[i]\[j] , i, j都是代表字符的位置

{% embed url="https://leetcode.cn/problems/longest-increasing-subsequence/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路: dp\[i] = 以s\[i]结尾的字串的最长递增子序列长度

{% embed url="https://leetcode.cn/problems/palindrome-partitioning-iii/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路：&#x20;
  * 拆分2个问题
    * 子串变成回文数的编辑次数 cost\[i]\[j]
    * 对字串s\[:i] 分割为 j 次的最小次数 dp\[i]\[j]
    * dp\[i]\[j]转移方程: dp\[i]\[j] 变成用 i1 遍历0 \~i 位置里的  上一个位置状态dp\[i1]\[j-1] + 最后一次编辑的次数cost\[i1]\[i-1]
  * code

{% code overflow="wrap" expandable="true" %}
```python
// Some code
class Solution:
    def palindromePartition(self, s: str, k: int) -> int:
        """
        拆分问题
        1. find palindrome first and use dp[i][j] = cost of modifying s[i:j+1] to palindrome
        2. The find  min cost
        """
        n = len(s)
        cost = [ [0] * len(s)  for _ in range(len(s))]
        # find  palindrome cost
        for gap in range(2, n+1):
            # gap between left, right boundary
            for left in range( n-gap +1):
                right = left + gap -1
                # 从左到右移动，gap从小到大变大
                # cost = 变成回文数的编辑次数
                cost[left][right] = cost[left+1][right-1] + 0 if s[left] == s[right] else 1
        
        dp = [ [10**9] * (k+1) for _ in range(n+1) ]
        dp[0][0] = 0
        # dp[i][j] = 对s[:i+1]切分成j个回文数的次数和 
        # dp[i][j] 依赖上一个相邻字串的总编辑次数 +  新增字串的cost
        for i in range(1, n+1):
            # 字串长度为i
            for j in range(1, min(k, i) +1):
                # 切分成 j个分区，j< k,i
                if j == 1:
                    # 只切分1个分区，就是 s[0: i]字串
                    dp[i][j] = cost[0][i-1]
                else:
                    for i1 in range(j-1, i):
                        # cost[i1][i-1] = 最后一个分区的编辑次数
                        # dp[i1][j-1] 前j-1个分区且末尾字串为i1的字串的编辑次数
                        dp[i][j] = min(dp[i][j] , dp[i1][j-1] + cost[i1][i-1])
        return dp[n][k]

```
{% endcode %}



{% embed url="https://leetcode.cn/problems/jump-game-ii/submissions/690667237/" %}

* 跳台阶: dp, 状态dp\[i]= i结尾最远达到距离， i 达不到max\_pos时需要step一跳



{% embed url="https://leetcode.cn/problems/longest-increasing-subsequence/" %}

* 思路: dp, 子序列问题 dp\[i] = 以i结尾的子序列的最长长度，&#x20;





{% embed url="https://leetcode.cn/problems/longest-palindromic-substring/solutions/63641/zhong-xin-kuo-san-fa-he-dong-tai-gui-hua-by-reedfa/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路: 找最长回文数， 中心延展法 内到外延申
  * 备注: 如果是检测是否回文数， 可以由外到内递归判断 dp\[i:j] = dp\[ i+1:j-1 ] && s\[i]==s\[j]



{% embed url="https://leetcode.cn/problems/target-sum/description/" %}



{% embed url="https://leetcode.cn/problems/contiguous-array/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路: prefix sum =0



{% embed url="https://leetcode.cn/problems/longest-arithmetic-subsequence-of-given-difference/description/?envType=problem-list-v2&envId=fkOSqLYV" %}



{% embed url="https://leetcode.cn/problems/longest-common-subsequence/?envType=problem-list-v2&envId=fkOSqLYV" %}



```
// Some code
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        """
            a b c d e       
        a   1 1 1 1 1 
        c   1 1 2 2 2
        e   1 1 2 2 3


            b s b i n i n m
        j   0 0 0 0 0 0 0 0 
        m   0 0 0 0 0 0 0 1
        j   0 0 0 0 0 0 0 1
        k   0 0 0 0 0 0 0 1
        b   1 1  
        k
        j
        k
        v
        dp[i][j] = s[:i] 和s2[:j] 最长公共子序列长度
        dp[i][j] = max(dp[i-1][j-1], dp[i-1][j], dp[i][j-1] ) + 0 if s[i]!=s[j] else 1 
        """
        #长度+1 是为了考虑边界值， 从空值开始
        dp = [ [0] * (len(text1)+1) for _ in range(len(text2)+1) ]

        for r in range(1, len(text2)+1):
            for c in range(1, len(text1)+1):
                if text1[c-1] == text2[r-1]:
                    # 如果最后一个字符是一样的，那么dp[r][c]就是前一行和前一列的解+1
                    dp[r][c] = dp[r-1][c-1] + 1
                else:
                    # 如果最后一个字符不一样, 那么就找上一次情况的解里面最大那个
                    dp[r][c] = max(max(dp[r-1][c], dp[r][c-1]), dp[r-1][c-1])

        return dp[-1][-1]
                
                    
class Solution:
    def longestCommonSubsequence(self, s: str, t: str) -> int:
        f = [0] * (len(t) + 1)
        for x in s:
            pre = 0  # f[0]
            for j, y in enumerate(t):
                tmp = f[j + 1]
                f[j + 1] = pre + 1 if x == y else max(f[j + 1], f[j])
                pre = tmp
        return f[-1]

作者：灵茶山艾府
链接：https://leetcode.cn/problems/longest-common-subsequence/solutions/2133188/jiao-ni-yi-bu-bu-si-kao-dong-tai-gui-hua-lbz5/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





## 待做题目

#### sorting&#x20;

quick sort

[https://app.gitbook.com/o/MQg1qdqArWYMTe4S9fPp/s/-MU4s3p9Ql9V0G1xvT9E-2910905616/datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method](../../datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method.md)

merge sort

[https://app.gitbook.com/o/MQg1qdqArWYMTe4S9fPp/s/-MU4s3p9Ql9V0G1xvT9E-2910905616/datastructure/sorting/mergesort-linkedlist](../../datastructure/sorting/mergesort-linkedlist.md)

heap sort

[https://app.gitbook.com/o/MQg1qdqArWYMTe4S9fPp/s/-MU4s3p9Ql9V0G1xvT9E-2910905616/datastructure/heap-and-queue/find-the-k-largest](../../datastructure/heap-and-queue/find-the-k-largest.md)

{% embed url="https://leetcode.cn/problems/kth-smallest-element-in-a-sorted-matrix/" %}

{% embed url="https://www.nowcoder.com/practice/fc897457408f4bbe9d3f87588f497729?tpId=196&rp=1&ru=%2Factivity%2Foj&qru=%2Fta%2Fjob-code-total%2Fquestion-ranking&tab=answerKey&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}



#### Searching

#### Array&#x20;

{% embed url="https://www.nowcoder.com/practice/37548e94a270412c8b9fb85643c8ccc2?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

{% embed url="https://www.nowcoder.com/practice/1624bc35a45c42c0bc17d17fa0cba788?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

{% embed url="https://leetcode.cn/problems/minimum-window-substring/" %}



DFS

BFS

#### DP&#x20;

{% embed url="https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/description/?envType=problem-list-v2&envId=fkOSqLYV" %}



{% embed url="https://leetcode.cn/problems/palindrome-partitioning-iii/description/?envType=problem-list-v2&envId=fkOSqLYV" %}



#### Tree

树遍历

{% embed url="https://leetcode.cn/problems/diameter-of-n-ary-tree/?envType=problem-list-v2&envId=fkOSqLYV" %}



#### Graph



