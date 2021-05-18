# User-base/Item-base实现

User-Base: 计算similarity matrix of user-user using cosine similarity

然后通过similarity matrix between user- user 来user vector之间的weighted sum来计算rating

```text
import pandas as pd
import numpy as np
def userCF(users, items):
  num_user = len(users.keys())
  num_item = len(items.keys())
  sim_matrix_user = pd.DataFrame(np.zeros((num_user,num_user)), index=users.keys(), columns=users.keys())
  for i in range(num_user):
    for j in range(i, num_user):
      intersec_items = []
      dot_prod = 0
      num_ui,num_uj = 0,0

      ui = sim_matrix_user.columns[i]
      uj = sim_matrix_user.columns[j]

      for item in items.keys():

        # using cosine similarity
        if item in users[ui].keys():
          num_ui += users[ui][item]**2
        if item in users[uj].keys():
          num_uj += users[uj][item]**2
        if item in users[ui].keys() and item in users[uj].keys():
          dot_prod += (users[ui][item] * users[uj][item])
      similarity = dot_prod/(np.sqrt(num_uj) * np.sqrt(num_ui))
      sim_matrix_user[ui][uj] = similarity
      sim_matrix_user[uj][ui] = similarity
  return sim_matrix_user





def user_Recommend(user, sim_matrix_user, users, items, k):
  # select top K similar users for selection
  similar_users = sim_matrix_user[user].sort_values(ascending = False)
  topk_users = similar_users[1:1+k]
  # dataframe storing result 
  rating_df = pd.DataFrame()
  user_rating = pd.DataFrame(users)
  w_sum = 0
  # find weighted sum of rating between input user and all item
  for u in topk_users.keys():
      rating_df = rating_df.append(topk_users[u]* user_rating[u])
      
  rating_df = (rating_df.sum()/sum(topk_users)).sort_values(ascending = False)
  return topk_users, rating_df


```

另外一种写法：

```text
# 基于用户的召回 u2u2i
def user_based_recommend(user_id, user_item_time_dict, u2u_sim, sim_user_topk, recall_item_num, 
                         item_topk_click, item_created_time_dict, emb_i2i_sim):
    """
        基于文章协同过滤的召回
        :param user_id: 用户id
        :param user_item_time_dict: 字典, 根据点击时间获取用户的点击文章序列   {user1: [(item1, time1), (item2, time2)..]...}
        :param u2u_sim: 字典，文章相似性矩阵
        :param sim_user_topk: 整数， 选择与当前用户最相似的前k个用户
        :param recall_item_num: 整数， 最后的召回文章数量
        :param item_topk_click: 列表，点击次数最多的文章列表，用户召回补全
        :param item_created_time_dict: 文章创建时间列表
        :param emb_i2i_sim: 字典基于内容embedding算的文章相似矩阵
        
        return: 召回的文章列表 [(item1, score1), (item2, score2)...]
    """
    # 历史交互
    user_item_time_list = user_item_time_dict[user_id]    #  [(item1, time1), (item2, time2)..]
    user_hist_items = set([i for i, t in user_item_time_list])   # 存在一个用户与某篇文章的多次交互， 这里得去重
    
    items_rank = {}
    for sim_u, wuv in sorted(u2u_sim[user_id].items(), key=lambda x: x[1], reverse=True)[:sim_user_topk]:
        for i, click_time in user_item_time_dict[sim_u]:
            if i in user_hist_items:
                continue
            items_rank.setdefault(i, 0)

            items_rank[i] += wuv
        
    # 热度补全
    if len(items_rank) < recall_item_num:
        for i, item in enumerate(item_topk_click):
            if item in items_rank.items(): # 填充的item应该不在原来的列表中
                continue
            items_rank[item] = - i - 100 # 随便给个复数就行
            if len(items_rank) == recall_item_num:
                break
        
    items_rank = sorted(items_rank.items(), key=lambda x: x[1], reverse=True)[:recall_item_num]    
    
    return items_rank
```





Item-Base: 计算similarity matrix of item-item using cosine similarity

然后通过similarity matrix between item- item 来计算item vector之间的weighted sum以及每个user对这个item的rating



```text

def itemCF(users, items):
  num_user = len(users.keys())
  num_item = len(items.keys())
  sim_matrix_item = pd.DataFrame(np.zeros((num_item,num_item)), index=items.keys(), columns=items.keys())
  for i in range(num_item):
    for j in range(i, num_item):
      #intersec_items = []
      dot_prod = 0
      num_ii,num_ij = 0,0

      ii = sim_matrix_item.columns[i]
      ij = sim_matrix_item.columns[j]

      for user in users.keys():
        
        # using cosine similarity
        if user in items[ii].keys():
          num_ii += items[ii][user]**2
        if user in items[ij].keys():
          num_ij += items[ij][user]**2
        if user in items[ii].keys() and user in items[ij].keys():
          dot_prod += (items[ii][user] * items[ij][user])
      similarity = dot_prod/(np.sqrt(num_ij) * np.sqrt(num_ii))
      sim_matrix_item[ii][ij] = similarity
      sim_matrix_item[ij][ii] = similarity
  return sim_matrix_item


def item_Recommend(item, sim_matrix_item, users, items, k):
  # select top K similar users for selection
  similar_items = sim_matrix_item[item].sort_values(ascending = False)
  topk_items = similar_items[1:1+k]
  # dataframe storing result 
  rating_df = pd.DataFrame()
  item_rating = pd.DataFrame(items)
  w_sum = 0
  # find weighted sum of rating between input user and all item
  for i in topk_items.keys():
      rating_df = rating_df.append(topk_items[i]* item_rating[i])
      
  rating_df = (rating_df.sum()/sum(topk_items)).sort_values(ascending = False)
  return topk_items, rating_df
```



另外一种写法：

```text
def item_based_recommend(user_id, user_item_time_dict, i2i_sim, sim_item_topk, recall_item_num, item_topk_click, item_created_time_dict, emb_i2i_sim):
    """
        基于文章协同过滤的召回
        :param user_id: 用户id
        :param user_item_time_dict: 字典, 根据点击时间获取用户的点击文章序列   {user1: [(item1, time1), (item2, time2)..]...}
        :param i2i_sim: 字典，文章相似性矩阵
        :param sim_item_topk: 整数， 选择与当前文章最相似的前k篇文章
        :param recall_item_num: 整数， 最后的召回文章数量
        :param item_topk_click: 列表，点击次数最多的文章列表，用户召回补全
        :param emb_i2i_sim: 字典基于内容embedding算的文章相似矩阵
        
        return: 召回的文章列表 [(item1, score1), (item2, score2)...]
    """
    # 获取用户历史交互的文章
    user_hist_items = user_item_time_dict[user_id]
    user_hist_items_ = {user_id for user_id, _ in user_hist_items}
    
    item_rank = {}
    for loc, (i, click_time) in enumerate(user_hist_items):
        for j, wij in sorted(i2i_sim[i].items(), key=lambda x: x[1], reverse=True)[:sim_item_topk]:
            if j in user_hist_items_:
                continue
            
            item_rank.setdefault(j, 0)
            item_rank[j] += wij
    
    # 不足10个，用热门商品补全
    if len(item_rank) < recall_item_num:
        for i, item in enumerate(item_topk_click):
            if item in item_rank.items(): # 填充的item应该不在原来的列表中
                continue
            item_rank[item] = - i - 100 # 随便给个负数就行
            if len(item_rank) == recall_item_num:
                break
    
    item_rank = sorted(item_rank.items(), key=lambda x: x[1], reverse=True)[:recall_item_num]
        
    return item_rank
```







