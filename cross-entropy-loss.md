# Cross Entropy Loss



Cross Entropy Loss implementation

```text
class CELoss(nn.Module):
  def __init__(self,num_class):
    super(CELoss, self).__init__()
    self.num_class = num_class
  def forward(self, target, y):
    #input: target: 1D number label list with values from 0 to num_class-1
    #y: a list of output vector, 2D matrix
    
    #convert target to one-hot
    n = len(target)
    # first create an eye matrix with size mxm, m = number of class, then use
    # slicing method to copy and extract each one-hot label to create one-hot label list
    # 
    one_hot = torch.eye(self.num_class)[s]
    loss = torch.sum(one_hot* torch.log(y),dim=1) # loss of each sample
    return torch.mean(loss)

n= 10
y = torch.rand(size = (n, 5))
y/= torch.sum(y,dim=1, keepdim=True)
target = torch.randint(low=0,high = 4,size = (n,))
celoss = CELoss(5)
y, target, celoss(target, y)
```







