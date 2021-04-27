# Deep&Cross network \(DCN\)







Implementation of DCN

```text
class CrossNet(nn.Module):
  def __init__(self, fea_dim = 5,embed_dim = 5, layer_num=3):
    super(CrossNet, self).__init__()
    self.weight = nn.Parameter(torch.randn(layer_num, fea_dim))
    self.bias = nn.Parameter(torch.randn(layer_num, fea_dim))
    self.layer_num = layer_num
  def forward(self, x):
    #
    #x: dense input  after concating embedding
    in_fea = x[:,:]
    # print(x.shape)
    for j in range(len(x)):
      x0 = x[j,:]
      for i in range(self.layer_num):
        in_fea[j,:] = torch.dot(x0,in_fea[j,:]) *self.weight[i,:] + self.bias[i,:] + in_fea[j,:]
        #print(in_fea[j,:].shape)

    # for i in range(self.layer_num):
    #   # dot product of vector weight[i,:] with all rows of input
    #   in_fea = torch.tensordot(in_fea,self.weight[i,:],dims=([1],[0]))
    #   in_fea = torch.multiply(in_fea,x)
    #   in_fea  += self.bias[i,:] + in_fea
    #   #print(in_fea[j,:].shape)
         
    return in_fea


class DCN(nn.Module):
  def __init__(self, fea_dim = 5,emb_dim=8, dense_dim =5):
    super(DCN, self).__init__()
    # using weight vector in each layer
    #embedding matrix
    self.V = nn.Parameter(torch.randn(fea_dim, emb_dim))
    
    dnn_input_size = emb_dim * fea_dim + dense_dim
    layers =[nn.Linear(dnn_input_size,256),
             nn.ReLU(),
             nn.Dropout(0.5)
             ]
    # layers += layers
    #layers += [nn.Linear(256,1)]
    self.DNN = nn.Sequential(*layers)

    self.CrossN = CrossNet(dnn_input_size, layer_num=3)
    self.linear = None#nn.Linear()

  def forward(self,x_dense, x_sparse):
    # 
    #convert feature to dense features
    embedding_list = []
    for i in range(len(x_sparse)):
      emb_vec = torch.multiply( torch.unsqueeze(x_sparse[i],1),self.V)
      emb_vec = torch.flatten(emb_vec)
      embedding_list.append(emb_vec)
    embedding_list = torch.stack(embedding_list)
    dense_input = torch.cat([embedding_list,x_dense],dim=1)
    cross_out = self.CrossN(dense_input)
    dnn_out = self.DNN(dense_input)
    #out = dnn_out + cross_out
    cat_out = torch.cat([dnn_out, cross_out],dim=1)
    if self.linear == None:
      self.linear = nn.Linear(cat_out.shape[1],1)
    out =self.linear(cat_out)
    print(dnn_out,cross_out)
    return out


fea_dim = 5
emb_dim=8
dense_dim =5
batch = 5
dcn = DCN(fea_dim = 5,emb_dim=8, dense_dim =5)

# sparse vectors
x_sparse = torch.randn(batch,fea_dim)<0.5

# dense vectors
x_dense = torch.randn(batch,dense_dim)
out = dcn(x_dense, x_sparse)
out 
```





