# Non-Negative Matrix Fatorization\(NMF\)

Link:

1.  [http://statweb.stanford.edu/~tibs/sta306bfiles/nnmf.pdf](http://statweb.stanford.edu/~tibs/sta306bfiles/nnmf.pdf)
2. [https://papers.nips.cc/paper/2000/file/f9d1152547c0bde01830b7e8bd60024c-Paper.pdf](https://papers.nips.cc/paper/2000/file/f9d1152547c0bde01830b7e8bd60024c-Paper.pdf)

NMF梯度的更新公式

$$
\theta_{V_k} = \frac{0.5*||U_k(V_k-V_{k+1})||^2_F}{0.5 * ||Vk -V_{k+1}||^2_F}
$$

$$
\theta_{U_k} = \frac{0.5*||V_{k+1}(U_k-U_{k+1})||^2_F}{0.5 * ||U_k -U_{k+1}||^2_F}
$$





```text
# integrate all functions together
class NMF:
  def __init__(self, M, U, V):
    # print("M:",M)
    # print("U:",U)
    # print("V:",V)
    self.M = M
    self.Uk = U
    self.Vk = V
    self.Uk1 = U
    self.Vk1 = V
  def update_parameter(self):
    self.Vk1 = self.Vk * np.matmul(self.Uk.transpose(),self.M)/np.matmul(self.Uk.transpose(),np.matmul(self.Uk,self.Vk))
    self.Uk1 = self.Uk * np.matmul(self.M,self.Vk1.transpose())/np.matmul(self.Uk,np.matmul(self.Vk1,self.Vk1.transpose()))
    return self.Vk1, self.Uk1
  def coeff_theta(self,Uk, Vk, Vk1):
    # Since 0.5*||Uk(Vk-V_k+1)||^2 = 0.5*theta||Vk-V_k+1||^2
    # theta  = ||Uk(Vk-V_k+1)||^2/||Vk-V_k+1||^2

    # Note: when ||Vk-Vk1||^2 =0, theta =norm/0.0 leading to invalid value
    # Besides, ||Vk-Vk1||^2 =0, 0.5*||Uk(Vk-V_k+1)||^2 = 0.5*theta||Vk-V_k+1||^2 =0
    # theta can be any value. In this case, return None
    norm =  np.sum(np.square(np.matmul(Uk, Vk-Vk1)))
    diff_vk_norm = np.sum(np.square(Vk-Vk1))
    if diff_vk_norm !=0.0:
      theta =norm /((diff_vk_norm))
    else:
      theta = None
    return theta
  def diff_norm(self, Uk, Vk, Vk1):
    # 0.5||Uk(Vk-V_k+1) ||^2
    return 0.5* np.sum(np.square(np.matmul(Uk, Vk-Vk1)))
  def innerProd(self, Uk, Vk, Vk1):
    # inner product (Vk1-Vk)^T(M^TUk-Vk1^TUk^TUk)
    return np.sum( np.transpose(Vk1-Vk) * (np.matmul(self.M.T,Uk) - np.matmul(Vk1.T,np.matmul(Uk.T,Uk)) ))

  def innerProd2(self, Uk, Vk, Vk1):
    # Using the formula I have proved 
    X= np.matmul(np.transpose(Uk), Uk)
    sum = 0.0
    for i in range(np.shape(Vk)[1]):
      vk = Vk[:,i]
      vk1 = Vk1[:,i]
      for z in range(np.shape(Vk)[0]):
        for j in range(np.shape(Vk)[0]):
          if j != z:
            tmp = X[z,j]*np.square(np.sqrt(vk[z]/vk[j])*(vk1[j]- vk[j]) - np.sqrt(vk[j]/vk[z])*(vk1[z]- vk[z]) )
            # print("loss: ",tmp)
            sum += tmp
    sum *= 0.5
    return sum
  def F_norm(self,U,V):

    return 0.5*np.sum(np.square(self.M-np.matmul(U,V)))
    
  
  
# Note
# inner_p2 is the inner product using the formula I proved:  0.5*sum_i(sum_j (sqrt(vi/vj)（vkj-vj）- sqrt(vj/vi)(vki-vi))^2 )
# inner_p1 is the original inner product from the paper <gradient(Uk, V_k+1), V_k - V_k+1>



U = np.random.random([15,5])      *10**0
V = (np.random.random([5,8])+1.0)    *10**0

#Choose one of two methods to generate M
# M = np.matmul(U, np.matmul(np.diag(np.random.random(5)),V))
M = np.random.random([15,8])      *10**0
iteration = 10000
print_times = 0 if iteration<3000 else iteration-3000

model = NMF(M,U,V)

singular_v = []
Uk_theta = []
Vk_theta = []
t = []
Err = []
diff_vk =[]
diff_g = []
inner_p2 = []
inner_p1 = []
conv_rate = []
Qk_ls = []
Pk=[]


for i in range(iteration):
  model.update_parameter()
  _,wu, _= np.linalg.svd(np.matmul(model.Uk.T,model.Uk))
  _,wv, _= np.linalg.svd(np.matmul(model.Vk.T,model.Vk))
  singular_v.append((min(wu),max(wu),min(wv),max(wv)))
  t.append(i)

  # Uk_theta = ||Uk(Vk-V_k+1)||^2 / 0.5*||Vk-V_k+1||^2
  uk_theta = model.coeff_theta(model.Uk, model.Vk,model.Vk1)
  if uk_theta == None:
    if len(Uk_theta)>0:
      uk_theta = Uk_theta[-1]
    else:
      uk_theta =0.0
  Uk_theta.append(uk_theta)

  # Vk_theta = ||V_k+1(Uk- U_k+1)||^2 /0.5*||Uk- U_k+1||^2
  vk_theta = model.coeff_theta(model.Vk1.T, model.Uk.T,model.Uk1.T)
  if vk_theta == None:
    if len(Vk_theta)>0:
      vk_theta = Vk_theta[-1]
    else:
      vk_theta =0.0
  Vk_theta.append(vk_theta)
  
  Qk_ls.append((model.Uk, model.Vk))
  Err.append(model.F_norm(model.Uk, model.Vk))
  #0.5||U(Vk-Vk1)||^2
  dif_vk = model.diff_norm(model.Uk,model.Vk,model.Vk1)
  diff_vk.append(dif_vk)

  # g(V_k) - g(V_k1)
  diff_g.append(model.F_norm(model.Uk, model.Vk) - model.F_norm(model.Uk, model.Vk1))  
  # two forms of inner product
  inner_p1.append(model.innerProd(model.Uk,model.Vk,model.Vk1))
  inner_p2.append(model.innerProd2(model.Uk,model.Vk,model.Vk1))
  model.Uk, model.Vk = model.Uk1, model.Vk1
  

# plt.plot(t, Err,'-')
# plt.plot(t, diff_vk, '-')
# plt.plot(t, inner_p1,'-')
# plt.plot(t, inner_p2,'-')
# plt.plot(t, diff_g,'-')
# plt.plot(t, conv_rate,'-')

plt.plot(t, np.log(Err),'-')
plt.plot(t, np.log(diff_vk), '-')
plt.plot(t, np.log(inner_p1),'-')
plt.plot(t, np.log(inner_p2),'-')
plt.plot(t, np.log(diff_g),'-')
plt.xlabel("time steps")
plt.ylabel("log loss")
plt.legend(["||M-UV||^2","0.5||U(Vk-Vk1)||^2","inner Product 1","inner Product 2" ,"g(Vk)-g(Vk1)"])
```

