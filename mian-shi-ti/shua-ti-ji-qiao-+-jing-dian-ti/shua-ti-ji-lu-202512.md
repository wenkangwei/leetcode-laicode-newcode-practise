# 刷题记录- 202512

## 已做题目

**sorting**

* quick sort/ quick selection

{% content-ref url="../../datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method.md" %}
[find-topk-largest-quickselect-kuai-su-xuan-ze-method.md](../../datastructure/sorting/find-topk-largest-quickselect-kuai-su-xuan-ze-method.md)
{% endcontent-ref %}

* 思路： divide and conquer. 每次找到start, end-1 范围内array\[end]适合插入的位置，该位置左边是大于array\[end]的所以元素， 右边是所有小于array\[end]的元素



**searching**

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



**heap/Array**

{% embed url="https://leetcode.cn/problems/merge-k-sorted-lists/" %}

* 思路: min-heap, 时间复杂度nlogk

{% embed url="https://www.nowcoder.com/practice/1624bc35a45c42c0bc17d17fa0cba788?tpId=188&rp=1&ru=%2Fta%2Fjob-code-high-week&qru=%2Fta%2Fjob-code-high-week&difficulty=&judgeStatus=&tags=&title=&sourceUrl=&gioEnter=menu" %}

* 滑动窗口+ 单调队列（单调递减队列存放window里第1，2，3.。k大值）



**DFS**



{% embed url="https://leetcode.cn/problems/permutations-ii/" %}

{% embed url="https://leetcode.cn/problems/permutations/submissions/688951631/" %}

* 思路： dfs + option排序去重逻辑



{% embed url="https://leetcode.cn/problems/combination-sum-iii/?envType=problem-list-v2&envId=fkOSqLYV" %}

* 思路：组合问题，不是排列问题， recursion 里只有2个child
* 备注： 细节问题， 注意传参被传错



**BFS**

{% embed url="https://www.nowcoder.com/practice/0c9664d1554e466aa107d899418e814e?tpId=188&tags=&title=&difficulty=&judgeStatus=&rp=1&sourceUrl=&gioEnter=menu" %}

* 思路： bfs， 遍历整个matrix， 每次找到element=1 就bfs queue。 注意：queue append要是当前位置的上下左右4个方向，而不是右下，因为queue append的方向并不是往右下角遍历



**Tree**

树遍历



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

{% embed url="https://leetcode.cn/problems/palindrome-partitioning/description/" %}

{% embed url="https://leetcode.cn/problems/palindrome-partitioning-iii/description/?envType=problem-list-v2&envId=fkOSqLYV" %}

BFS



#### DP&#x20;

{% embed url="https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/description/?envType=problem-list-v2&envId=fkOSqLYV" %}

####

#### Tree

树遍历

{% embed url="https://leetcode.cn/problems/diameter-of-n-ary-tree/?envType=problem-list-v2&envId=fkOSqLYV" %}



#### Graph



