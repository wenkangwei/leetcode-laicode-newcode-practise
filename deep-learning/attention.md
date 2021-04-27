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
        q = torch.matmul(x, self.wq)
        k = torch.matmul(x, self.wk)
        v = torch.matmul(x, self.wv)
        a = torch.softmax(torch.matmul(q, k.T)/emb_dim**-0.5, dim=1) # do softmax across row
        
        z = torch.matmul(a, v)
        return z

size=8
n=10
emb_dim = 5
x = torch.randn([n,size],requires_grad=False)
att = SelfAttention()
z = att(x)
z.shape
```

