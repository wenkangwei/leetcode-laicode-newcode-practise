# Attention

Implementation of Self-attention

```python
# Self-Attention
#torch.matmul(): matrix multiplication
# torch.multiply(): element wise multiplication
import torch
from torch import nn
class SelfAttention(nn.Module):
    def __init__(self, size=8, n=10, emb_dim = 5):
        super(SelfAttention,self).__init__()
        self.wq = torch.randn([size, emb_dim])
        self.wk = torch.randn([size, emb_dim])
        self.wv = torch.randn([size, emb_dim])
        
    def forward(self,x):
        res = []
        for i in range(len(x)):
          xi = x[i]
          q = torch.matmul(xi, self.wq)
          k = torch.matmul(xi, self.wk)
          v = torch.matmul(xi, self.wv)
          a = torch.softmax(torch.matmul(q, k.T)/emb_dim**-0.5, dim=1) # do softmax across row
          
          z = torch.matmul(a, v)
          res.append(z)
        out = torch.stack(res)
        return out

size=8 # width of a sample
depth = 3 # number of embedding vec or num of patch in a sample
n=10 # number of samples
emb_dim = 5 # embedding vec size

x = torch.randn([n,depth,size],requires_grad=False)
att = SelfAttention(size=8, n=10, emb_dim = 5)
z = att(x)
z.shape,z
```

Reference

[https://blog.csdn.net/qq\_37394634/article/details/102679096](https://blog.csdn.net/qq_37394634/article/details/102679096)

