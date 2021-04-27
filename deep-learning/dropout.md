# Dropout

Dropout 实现

```text
class Dropout(torch.nn.Module):
  def __init__(self, p=0.5):
    super(Dropout, self).__init__()
    self.p = p
    pass
  def forward(self, x):
    s = x.shape
    dropout_mask = (torch.rand(*s)<self.p)/self.p   # divided by p to rescale  weight to avoid output range changes
    #print(dropout_mask)
    out = x* dropout_mask # element-wise multiplication 
    return out

size = 6
batch_size = 3
x = torch.tensor([[1.1, 1, 2, 3, 4, 5],[10, 11, 12, 13, 14, 15],[21,22,23,24,25,26]], dtype= torch.float32)
linear = nn.Linear(6, 6)
relu = nn.ReLU()

dropout = Dropout(1)
x = linear(x)
x = relu(x)
#print("relu:", x)
x = dropout(x)
x
```





