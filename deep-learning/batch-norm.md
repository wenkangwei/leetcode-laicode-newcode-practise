# Batch Norm

**Paper**: [https://arxiv.org/pdf/1502.03167.pdf](https://arxiv.org/pdf/1502.03167.pdf)

**Batch Normalization transformation:** 

![](../.gitbook/assets/image%20%284%29.png)

**Training:**

when training update gamma and beta using batch samples and also update running mean E\[x\], running variance Var\[x\]

When inference, use running mean E\[x\], running variance Var\[x\], rather than sample mean and sample var, to compute output

![](../.gitbook/assets/image%20%285%29.png)

**Gradient Computation** during training and updating var, mean

![](../.gitbook/assets/image%20%286%29.png)



**Coding in pytorch**

```text
import torch
from torch import nn
class BatchNorm(nn.Module):
  def __init__(self, in_dim, out_dim):
    super(BatchNorm, self).__init__()
    self.linear = nn.Linear(in_dim, out_dim,bias=True)
    self.in_dim, self.out_dim = in_dim, out_dim
    self.eps = 0.001
    self.momentum = 0.95
    self.running_mean = torch.zeros(in_dim,dtype=torch.float32)
    self.running_var = torch.zeros(in_dim,dtype=torch.float32)
    pass
  def forward(self,x, inference = False):
    #normalize x according to batch 
    mu = torch.mean(x, dim=0, keepdim=True) # compute along the first dim: num of samples
    var = torch.var(x, dim=0,keepdim=True)  # compute along the first dim: num of samples
    
    #print("mean: ", mu)
    #print("var: ", var)
    if not inference:
      # when training, update running mean and var
      # update x with sample mean
      self.running_mean = self.momentum*self.running_mean + (1-self.momentum)*mu # update running mean with sample mean
      self.running_var = self.momentum*self.running_var + (1-self.momentum)*var # update running var with sample var
      x = (x-mu)/torch.sqrt(var + self.eps)
    else:
      # when inference, use estimated running mean, var
      x = (x - self.running_mean)/ torch.sqrt(self.running_var + self.eps)
    # linear projection to rescale and enhance feature
    out = self.linear(x)
    return out
x = torch.tensor([[1.1, 1, 2, 3, 4, 5],[10, 11, 12, 13, 14, 15],[21,22,23,24,25,26]], dtype= torch.float32)
x2 = torch.rand((3, 6),dtype = torch.float)
bn = BatchNorm(in_dim=6, out_dim=3)

bn(x), bn(x2),bn(x, inference=True)


```

