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



