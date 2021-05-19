# XGBoost and LGBM

## GDBT介绍









## XGBoost 介绍



## XGBoost 原理

### XGBoost的建树的步骤分为以下几步：

1. 定义目标函数Objective function
2. 通过泰勒展开式对Objective function进行简化
3. 对简化后的Objective function进行求解得到树的每个leaf node输出的值的计算公式，以及对树结构的评估函数
4. 根据所得到的评估函数用Exact Greedy algorithm 或者 近邻算法估计得到每个feature的分裂点以及对应的gain，并按照分裂点和gain对feature进行选取和分裂建树

### XGBoost 推导

**1. Objective function 优化目标函数：**

![XGBoost &#x76EE;&#x6807;&#x51FD;&#x6570;](../.gitbook/assets/image%20%2825%29.png)

在XGBoost的目标函数里面，它是基于GBDT的目标函数再加上一个 $$\sum_i^t\Omega(f_i) $$ 的对t棵树的输出regularization term正则化来抑制过拟合的问题. 而前面的loss 函数还是和之前一样 $$y_i$$ 是target $$\bar{y_i}^{(t)}$$ 是对前面t棵树的输出的线性相加的输出值。 在对第t棵树的输出进行计算时， 我们可以把第t棵树的regularization term和 预测值拆开来得到以上表达式。其中

![](../.gitbook/assets/image%20%2826%29.png)

而regularization term  $$\Omega(f_t) = \gamma T  + \frac{1}{2}||w||^2$$ 这里的gamm是一个penalty的系数而T是第t棵树 $$f_t$$ 的节点个数，w是这颗树的每个leaf node的输出预测。

**2. 通过泰勒展示简化 Objective function 优化目标函数：**

在XGBoost 里面，为了加速算法的计算，这里用了2阶的泰勒展开式对Objective function进行估计，泰勒展开式公式如下

![](../.gitbook/assets/image%20%2835%29.png)

之后我们可以把原来的目标函数简化成下面形式：

![](../.gitbook/assets/image%20%2822%29.png)

由于计算第t棵树的时候，前面t-1棵树的loss还有regularization的项是常数，我们可以不考虑。

![](../.gitbook/assets/image%20%2815%29.png)

把 $$f_t(x_i)$$ 树的输出替换成w的形式，我们得到关于w的一元二次函数

![](../.gitbook/assets/image%20%2834%29.png)



**3. 通过对关于w的一元二次方程求解可得**

由于我们的目标是要找出第t棵树使得我们当前的目标函数最小化，所以我们可以把上面的一元二次方程函数通过找极值的方法找到w的解以及对应的目标函数值，即 $$x = - \frac{b}{2a},  y = \frac{4ac-b^2}{4a}$$  因此我们可以得到以下的解

$$
w^*_j = - \frac{\sum_{i\in I_j} g_i}{\sum_{i\in I_j} h_i + \lambda}
$$

$$
OBj^{t}（q(x)） = -\frac{1}{2} \sum_{j=1}^T\frac{(\sum_{i\in I_j} g_i )^2}{\sum_{i\in I_j} h_i + \lambda} + \gamma T
$$

这里的 $$I_j$$ 是指属于第j个叶节点的样本的集合，而 $$g_i , h_i$$ 分别是关于之前t-1棵树的模型对第i个样本的输出的Loss的1阶和2阶的导数。  这里的目标函数 $$OBj^t(q(x))$$ 的值可以看成是对当前的第t棵树的loss，而q\(x\)代表了第t棵树的结构。



**4.  根据前面的OBj score 的函数 对当前节点下的所有feature查找最大的gain，并根据概念找到每个feature的split 分裂点来建树**

虽然计算第t棵树的每个leaf node的输出值的公式是有了， 但是我们其实还没有确定这棵树的结构。为了找到树的结构，我们需要给每个特征找到分裂点，以及在每一层对特征选取。而这里就需要用到第3步找到的leaf node 输出的值和目标函数值函数了。

这里举个例子， 在下图，假设我们先只考虑黄色看看的树。如果我们一开始还没建树，需要先选取一个feature \(1个column\)用来作为一个root node。那么为了使目标函数最小化我们最直接的方法是直接这个feature里面的出现的数值排序，然后把每两个值之间的间隙当成分裂点，然后看哪个分离点对应的OBj\(q\(x\)\)目标函数是最小就选那个作为当前的分裂点。所以这样在这个例子我们的分裂点是 x&lt;=6 , x&gt;6。这时我们的loss就是 $$L^{old}$$ 

![](../.gitbook/assets/image%20%2811%29.png)

由于我们想要

### XGBoost 特点



### Reference





