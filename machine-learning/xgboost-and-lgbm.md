# XGBoost

## GDBT介绍

GBDT——Gradient Boosting Decision Tree，又称Multiple Additive Regression Tree，后面这种叫法不常看到，但Regression Tree却揭示了GBDT的本质——GBDT中的树都是回归树。GBDT的主要是思想是每次把上一棵树的输出和label的残差作为下一棵树的label进行拟合，前面的树做不好的地方后面的补上，不断把拟合后的预测的残差值叠加上去，进行梯度的提升。具体步骤如下

![](../.gitbook/assets/image%20%2813%29.png)

其中负梯度相当于残差，用做target给下个tree进行拟合

![](../.gitbook/assets/image%20%2812%29.png)



GBDT的正则化防止过拟合的方法，主要有以下措施：

1，Early Stopping，本质是在某项指标达标后就停止训练\(比如树的高度之类的\)，也就是设定了训练的轮数；

2，Shrinkage，其实就是学习率，具体做法是将每课树的效果进行缩减，比如说将每棵树的结果乘以0.1，也就是降低单棵树的决策权重，相信集体决策；

3，Subsampling，无放回抽样，具体含义是每轮训练随机使用部分训练样本，其实这里是借鉴了随机森林的思想；

4，Dropout，这个方法是论文里的方法，没有在SKLearn中看到实现，做法是每棵树拟合的不是之前全部树ensemble后的残差，而是随机挑选一些树的残差

## XGBoost 介绍

XGBoost是陈天奇等人开发的一个开源机器学习项目，高效地实现了GBDT算 法并进行了算法和工程上的许多改进，被广泛应用在Kaggle竞赛及其他许多机器 学习竞赛中并取得了不错的成绩。

## XGBoost 原理

### XGBoost的建树的步骤分为以下几步：

1. 定义目标函数Objective function
2. 通过泰勒展开式对Objective function进行简化
3. 对简化后的Objective function进行求解得到树的每个leaf node输出的值的计算公式，以及对树结构的评估函数
4. 根据所得到的评估函数用Exact Greedy algorithm 或者 近邻算法估计得到每个feature的分裂点以及对应的gain，并按照分裂点和gain对feature进行选取和分裂建树

### XGBoost 推导

**1. Objective function 优化目标函数：**

![XGBoost &#x76EE;&#x6807;&#x51FD;&#x6570;](../.gitbook/assets/image%20%2828%29.png)

在XGBoost的目标函数里面，它是基于GBDT的目标函数再加上一个 $$\sum_i^t\Omega(f_i) $$ 的对t棵树的输出regularization term正则化来抑制过拟合的问题. 而前面的loss 函数还是和之前一样 $$y_i$$ 是target $$\bar{y_i}^{(t)}$$ 是对前面t棵树的输出的线性相加的输出值。 在对第t棵树的输出进行计算时， 我们可以把第t棵树的regularization term和 预测值拆开来得到以上表达式。其中

![](../.gitbook/assets/image%20%2829%29.png)

而regularization term  $$\Omega(f_t) = \gamma T  + \frac{1}{2}||w||^2$$ 这里的gamm是一个penalty的系数而T是第t棵树 $$f_t$$ 的节点个数，w是这颗树的每个leaf node的输出预测。

**2. 通过泰勒展示简化 Objective function 优化目标函数：**

在XGBoost 里面，为了加速算法的计算，这里用了2阶的泰勒展开式对Objective function进行估计，泰勒展开式公式如下

![](../.gitbook/assets/image%20%2838%29.png)

之后我们可以把原来的目标函数简化成下面形式：

![](../.gitbook/assets/image%20%2825%29.png)

由于计算第t棵树的时候，前面t-1棵树的loss还有regularization的项是常数，我们可以不考虑。

![](../.gitbook/assets/image%20%2818%29.png)

把 $$f_t(x_i)$$ 树的输出替换成w的形式，我们得到关于w的一元二次函数

![](../.gitbook/assets/image%20%2837%29.png)



**3. 通过对关于w的一元二次方程求解可得**

由于我们的目标是要找出第t棵树使得我们当前的目标函数最小化，所以我们可以把上面的一元二次方程函数通过找极值的方法找到w的解以及对应的目标函数值，即 $$x = - \frac{b}{2a},  y = \frac{4ac-b^2}{4a}$$  因此我们可以得到以下的解

$$
w^*_j = - \frac{\sum_{i\in I_j} g_i}{\sum_{i\in I_j} h_i + \lambda}
$$

$$
OBj^{t}（q(x)） = -\frac{1}{2} \sum_{j=1}^T\frac{(\sum_{i\in I_j} g_i )^2}{\sum_{i\in I_j} h_i + \lambda} + \gamma T
$$

这里的 $$I_j$$ 是指属于第j个叶节点的样本的集合，wj 是第j个节点的输出值，而 $$g_i , h_i$$ 分别是关于之前t-1棵树的模型对第i个样本的输出的Loss的1阶和2阶的导数。  这里的目标函数 $$OBj^t(q(x))$$ 的值可以看成是对当前的第t棵树的loss，而q\(x\)代表了第t棵树的结构。



**4.  根据前面的OBj score 的函数 对当前节点下的所有feature查找最大的gain，并根据概念找到每个feature的split 分裂点来建树**

虽然计算第t棵树的每个leaf node的输出值的公式是有了， 但是我们其实还没有确定这棵树的结构。为了找到树的结构，我们需要给每个特征找到分裂点，以及在每一层对特征选取。而这里就需要用到第3步找到的leaf node 输出的值和目标函数值函数了。

这里举个例子， 在下图，假设我们先只考虑黄色看看的树。如果我们一开始还没建树，需要先选取一个feature \(1个column\)用来作为一个root node。那么为了使目标函数最小化我们最直接的方法是直接这个feature里面的出现的数值排序，然后把每两个值之间的间隙当成分裂点，然后看哪个分离点对应的OBj\(q\(x\)\)目标函数是最小就选那个作为当前的分裂点。所以这样在这个例子我们的分裂点是 x&lt;=6 , x&gt;6。这时我们的loss就是 $$L^{old}$$ = OBj\(q\(x\)\) of old tree structure

![](../.gitbook/assets/image%20%2811%29.png)

由于我们想要通过添加剩下的feature作为tree的分支使得tree的loss 越小越好，那么对于old的框框下增加branch之后的tree的结构所对应的loss，标记为 $$L^{new}$$ = = OBj\(q\(x\)\) of new tree structure。 我们就想要 $$max(L^{old} - L^{new}) = OBj^{old} - OBj^{new}$$

而我们可以标记 $$L^{split} = max(L^{old} - L^{new})$$作为分裂增益， split gain。split gain公式可以变成

![](../.gitbook/assets/image%20%2841%29.png)

之后我们可以找到当前可以选的feature所对应的最大split gain以及对应的分裂点，并选取split gain最大的那个feature作为当前的tree node分支。 不断重复这几步就可以把树建起来了。 

而这个每次都要线性扫描分裂点的建树的方法也叫做  Exact Greedy Algorithm。在当前的节点里面，把归到这个节点里面的样本做以下操作：

![](../.gitbook/assets/image%20%2842%29.png)

由于这个方法每次都要把feature的数值排序并线性扫描，在计算连续特征的时候还要先排序并把每两个值的中点作为分裂点候选点来离散化，这样计算会很慢并且会在大量数据时内存容易不足。在XGBoost里面用了一种 近似算法\(Approximate algorithm\)按照预定的百分位对数值进行分桶得到候选的分裂点集合，然后再用Exact Greedy algorithm 进行分裂点选取。以下是原paper的描述：

![](../.gitbook/assets/image%20%2814%29.png)

例子：

![](../.gitbook/assets/image%20%2843%29.png)



### XGBoost 特点

GBDT：

1. GBDT 它的非线性变换比较多，表达能力强，而且不需要做复杂的特征工程和特征变换。
2. GBDT 的缺点也很明显，Boost 是一个串行过程，不好并行化，而且计算复杂度高，同时不太适合高维稀疏特征；
3. 传统 GBDT 在优化时只用到一阶导数信息。

Xgboost 它有以下几个优良的特性：

1. 显示的把树模型复杂度作为正则项加到优化目标中。
2. 公式推导中用到了二阶导数，用了二阶泰勒展开。
3. 实现了分裂点寻找近似算法。
4. 利用了特征的稀疏性。
5. 不同columns的数据事先排序并且以 block 形式存储，有利于用并行计算加速。
6. 传统GBDT用了所有数据，但XGBoost像RandomForest一样用了Column Subsampling 来抑制overfitting并且加速了计算。
7. 传统的GBDT没有设计对缺失值进行处理需要人工对缺失值处理，XGBoost能够自动学习出缺 失值的处理策略。
8. 树模型对outlier不敏感，不像Parameterized model,如LR之类的

其他特点：

1. GBDT 用的CART tree分类时用的是Gini gain 而回归时用方差 variance，而XGBoost里面的启发函数用的是一个基于前面树的梯度的一阶二阶导数的Loss



### Reference

\[1\] [https://zhuanlan.zhihu.com/p/143009353](https://zhuanlan.zhihu.com/p/143009353)

\[2\] [https://zhuanlan.zhihu.com/p/58269560](https://zhuanlan.zhihu.com/p/58269560)

\[3\] [https://zhuanlan.zhihu.com/p/53980138](https://zhuanlan.zhihu.com/p/53980138)



