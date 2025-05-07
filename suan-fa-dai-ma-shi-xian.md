# 算法代码实现

## Metric

```python
// Some code
class Metrics:
    def cal_auc(self, y_pred, y_true):
        """
        y_true: 1D- list
        y_pred: 1D-list
        auc = (number of positive pred > negative pred pair) / all pair conditions
        """
        pos_ls= []
        neg_ls = []
        for i in range(len(y_pred)):
            if y_true[i] != 0:
                pos_ls.append(i)
            else:
                neg_ls.append(i)

        cnt = 0
        for p in pos_ls:
            for n in neg_ls:
                if y_pred[p] > y_pred[n]:
                    cnt += 1
        auc = cnt/(len(neg_ls) * len(pos_ls))
        return auc
    def mrr(self, y_pred, y_true, topN=3):
        """
        Mean Reciprocal Rank
        强调的是用户的需求项在模型推荐列表中的位置，越靠前越佳。
        MRR = 1/S sum_i (1/pi), pi = i^th item rank position, S= sample amount
        pi = 打分的商品列表中第i个商品所在的排序后列表的位置。 如果第i个商品不在排序后的列表的topN
        的位置， 那么就是 1/pi=0
        y_pred: 1D- list
        y_true: 1D - list  with target item 
        """
        # 倒排
        rank = len(y_pred) - np.argsort(np.array(y_pred)) -1
        print("mrr rank:",rank,y_pred)
        topN_rank = rank.squeeze()[:topN]
        mrr = 0
        for i in y_true:
            if  i not in topN_rank:
                pi=0
            else:
                pi = 1/ (1+topN_rank.tolist()[i])
            mrr += pi
        return mrr
    def ndcg(self, y_preds, y_trues, K=5):
        """
        inputs: 
            y_preds: N*L
            y_trues: N*L
        y_pred仅仅用来算dcg的序列， 不需要它的score 绝对值！！
        normalize discount cumulative gain
        ndcg = dcg/idcg
        dcg = sum_i^N ( rel_i / log_2(i+1) )
        1. 先把y_pred 倒排，取topK, 以及找他们对应的ground truth score
        2. 用ground truth score 作为rel， 以及用pred排的序的位置rank作为 分母算dcg
        3. 算idcg
        4. ndcg = mean_i^N dcg/idcg 对N条样本的ndcg求均值
        """
        def dcg(pred, K):
            pred= pred[:K] # topK  ground truth relevence scores
            dcg=sum([ s/(np.log2(2+ i)) for i, s in enumerate(pred) ])
            return dcg
        res = 0
        for y_pred, y_true in zip(y_preds, y_trues):
            sorted_true_rel = [ x for _, x in sorted(zip(y_pred,y_true), reverse=True) ]
            dcg_val = dcg(sorted_true_rel, K)
            ideal_true_rel = sorted(y_true, reverse=True)
            idcg_val = dcg(ideal_true_rel, K)
            print("idcg: ", idcg_val)
            if idcg_val >0:
                ndcg = dcg_val/idcg_val 
            else:
                ndcg = 0
            res += ndcg
        res /= len(y_preds)
        return res
    def recall(self, y_pred, y_true, k):
        """
        recall = TP / (TP + FN)
        """
        def rc(pred, lb,k):
            sorted_lb = [ l for p, l in sorted(zip(pred, lb))]
            topk = sorted_lb[:k]
            return sum(topk)/sum(lb)
        res= []
        for pred, lb in zip(y_pred,y_true):
            res.append(rc(pred, lb, k))
        #  average recall
        return sum(res)/len(res)
    
    def precision(self, y_pred, y_true, K):
        def pk(pred, lb,k):
            sorted_lb = [ l for p, l in sorted(zip(pred, lb))]
            topk = sorted_lb[:k]
            return sum(topk), sum(topk)/k
        res= []
        x=0
        for pred, lb in zip(y_pred,y_true):
            num, pk_v = pk(pred, lb, K)
            res.append(pk_v)
            x+= num
        #  average recall
        return sum(res)/len(res), x/(len(y_pred)*K)
    
    def hitrate(self, y_pred, y_true, K):
        """
        hitrate@k: 分用户推荐的item的top k 是否有ground truth
        用户力度
        input:
            y_pred: N*L  N= number of user
            y_true: N*L
        https://cran.r-project.org/web/packages/recometrics/vignettes/Evaluating_recommender_systems.html
        """
        def hit(pred, lb, k):
            res = []
            for i, (p, l) in enumerate(sorted(zip( pred, lb), reverse=True)):
                if i <k:
                    res.append(l)
            return sum(res)>0
        hr = []
        for p, l in zip(y_pred, y_true):
            hr.append(hit(p, l,K))
        return sum(hr)/len(hr)
    
mt=Metrics()
print("metric auc:",mt.cal_auc([0.8,0.1,0.4,0.5,0.6,0.1],[1,0,1,0,0,0]))
print("metric mrr:",mt.mrr([0.8,0.1,0.4,0.5,0.6,0.1],[3, 2],4))
print("metric ndcg:",mt.ndcg( [[0.8,0.1,0.4,0.5,0.6,0.1], [0.8,0.1,0.4,0.5,0.6,0.1]],[[1,0,1,0,0,0], [1,0,1,1,0,0]] ,4))
print("metric hitrate:",mt.hitrate( [[0.8,0.1,0.4,0.5,0.6,0.1], [0.8,0.1,0.4,0.5,0.6,0.1]],[[0,0,0,0,0,1], [1,0,1,1,0,0]] ,4))
print("metric recall:",mt.recall( [[0.8,0.1,0.4,0.5,0.6,0.1], [0.8,0.1,0.4,0.5,0.6,0.1]],[[0,0,0,0,0,1], [1,0,1,1,0,0]] ,10))
print("metric precision:",mt.precision( [[0.8,0.1,0.4,0.5,0.6,0.1], [0.8,0.1,0.4,0.5,0.6,0.1]],[[0,0,0,0,0,1], [1,0,1,1,0,0]] ,10))
```
