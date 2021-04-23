# Attention

Implementation of Self-attention

```text
# Attention
#torch.matmul(): matrix multiplication
# torch.multiply(): element wise multiplication
size, n, emb_dim = 8, 10, 5
x = torch.randn([n,size])
wq = torch.randn([size, emb_dim])
wk = torch.randn([size, emb_dim])
wv = torch.randn([size, emb_dim])


q = torch.matmul(x, wq)
k = torch.matmul(x, wk)
v = torch.matmul(x, wv)
a = torch.softmax(torch.matmul(q, k.T)/emb_dim**-0.5, dim=1) # do softmax across row

z = torch.matmul(a, v)
z.shape, v.shape
```

