# DeepFM

paper: [https://www.ijcai.org/Proceedings/2017/0239.pdf](https://www.ijcai.org/Proceedings/2017/0239.pdf)

Implementation of Deep FM with more than 1 fields and each field has only 1 feature

```text
class FM(nn.Module):
  def __init__(self,fea_dim = 5,emb_dim=8):
    super(FM,self).__init__()
    # latent embedding vectors
    # each row = embedding vector of one feature 
    self.V = torch.randn(size = (fea_dim, emb_dim), dtype = torch.float32)
    self.linear = nn.Linear(fea_dim,1, bias=True)
  def forward(self, x):
    # input x: (batch size, feature dimension or fields num if using DeepFM)
    # linear logit part
    linear_out = self.linear(x)
    # second order part
    # vector = (\sum^n_i V_ix_i)^2 - \sum^n_i V_i^2 * x_i^2
    sum_square = torch.square(torch.matmul(x, self.V))
    square_sum = torch.matmul(torch.square(x), torch.square(self.V))
    cross_vec = 0.5*(sum_square - square_sum) # each row in matrix = crossing vector
    return linear_out, cross_vec



class DeepFM(nn.Module):
  def __init__(self, field_dim = 5,emb_dim=8):
    # DeepFm consists of
    # 1. embedding matrix for each field
    # 2. convert each field to one embedding vector
    # 3. FM for computing feature crossing
    # 4. Convert embedding to input samples for DNN
    # 3. DNN and input stacking embedding inputs
    #
    super(DeepFM, self).__init__()
    self.FM = FM(field_dim,emb_dim)
    dnn_input_size = emb_dim * field_dim
    layers =[nn.Linear(dnn_input_size,256),
             nn.ReLU(),
             nn.Dropout(0.5)
             ]
    # layers += layers
    layers += [nn.Linear(256,1)]
    self.DNN = nn.Sequential(*layers)
  def forward(self, x):
    # x: (batch size, number of features)
    # each sample contain m embedding vectors, each represents one field
    linear_out, cross_vec = self.FM(x)
    cross_out = torch.sum(cross_vec,dim=1)

    # extend embedding vectors from FM 
    embedding_list = []
    for i in range(len(x)):
      tmp = torch.unsqueeze(x[i], 0).T
      # compute weighted embedding vector
      vecs = torch.multiply(self.FM.V, tmp)
      # extend embedding vectors to get a concatenated embedding vectors
      # for DNN input
      vecs = torch.flatten(vecs)
      embedding_list.append(vecs)
    # convert list of tensor to a tensor matrix as dnn input
    embedding_list = torch.stack(embedding_list)
    # print(embedding_list.shape)
    print(linear_out.shape, cross_out.shape )
    out = torch.tensor(0, dtype = torch.float32)
    dnn_logit = self.DNN(embedding_list)
    print(dnn_logit.shape)
    
    linear_out= torch.squeeze(linear_out)
    dnn_logit= torch.squeeze(dnn_logit)
    out = linear_out + cross_out + dnn_logit
    
    return torch.sigmoid(out)


field_dim = 5
emb_dim = 8
dfm = DeepFM(field_dim,emb_dim)

# there are 5 fields and each field has only 1 feature
x = torch.rand(size=(3,field_dim))
out = dfm(x)
out
```



