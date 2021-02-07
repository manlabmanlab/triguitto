# primos
Pri
def pri(n):
  Primos=[]
  no_primos=[]
  for i in range(2,n+1):
    if i not in no_primos:
      primos.append(i)
    for j in range(i*i,n+1,i):
      no_primos.append(j)
  return primos

