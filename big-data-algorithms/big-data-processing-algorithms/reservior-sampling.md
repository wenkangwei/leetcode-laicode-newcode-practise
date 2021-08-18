# Reservior Sampling

## **Problem to solve**

在Big Data 的Random Sampling里面， 我们遇到的问题是**数据量太大以至于不能知道所有数据sample的总量**，并且不能直接把所有数据都存放到一个固定的buffer里面进行直接随机采样。

比如假设我们有一个容量为k的 Reservoir 储水池，我们要把n个数随机地采样选k个到储水池里面。这个问题我们可以这么看， 我们有一个容量为n的水槽， 这个水槽里面的容量为k的部分是用作储水池保留sample。 一个新的sample到来时，站在sample的角度，**这个sample X 在采样时被随机放到储水池的概率 / 被保留的概率 = P\(X 被保留\)  = k/n**

问题就是这个n我们是不知道的，不能直接用来做random sampling。 我们只知道目前进来的sample的个数即 k+ i \(在储水池满了之后\)。 因此，我们在**Reservior Sampling的思路就是我们要在新样本 X 不断进来时我们要一直保持 储水池中sample X的保留概率 P\(新样本X 被保留\)  = k/\(k+i \)， 直到 k+i =n 为止，我们就得到 P\(新样本X在采样时被保留\) = k/n**

## **Idea**

**Resevior Sampling 的做法步骤如下:**

1. **定义Reservior 容量为k， 先把data stream里面前k 个值填满 buffer**
2. **记录目前已经到来的sample个数 k+ i**
3. 随机在 range  \[1, k+i\] 里面选取一个index值
4. 如果index &lt;=k , 即储水池里面的对应index值的sample要被新进来的sample取代，所以 reservior的第k+i个sample 要被 保留的概率 = k/\(k+i\)
5. Repeat 直到所有数据 （n个数）被遍历完

**Resevior Sampling 的每个sample被保留的概率是一样的proof:**

1. 定义Reservoir储水池有 容量k, reservoir第j个sample 为 xj. 而新进来的第i个sample 为 xi, 而i&gt; k. 那么 **在第i个sample xi到来时 P\(xi 被保留\) = k/i** 
2.  P\(xj 被xi取代\) = P\(xj 被选中\) \* P\(xi 被保留\) = 1/k \* k/i = 1/i 
3. 同理在第 i+1 个sample 到来时， 第i个sample被 第i个sample取代的概率为 1/\(i+1\)
4. 那么在第 i个 sample之前被保留的情况下 **在第i+1 个sample 到来时**                     **P\(xi 被保留\)**  = P\(第i个sample**之前**被保留\) \* P\(第i个sample不被取代\) = k/i \*\(1- 1/\(i+1\)\) = **k/\(i+1\)**

## **Coding**

\*\*\*\*

## **Reference**

**\[1\]** [**https://www.cnblogs.com/ECJTUACM-873284962/p/6910842.html\#\_label6**](https://www.cnblogs.com/ECJTUACM-873284962/p/6910842.html#_label6)\*\*\*\*







