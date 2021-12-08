
创建向量和矩阵  
```python 
import numpy as np 

x = np.array([1, 2, 3, 4]) 
y = np.array([5, 6, 7, 8]) 
a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])  
```


axis；0 纵向，1 横向    
```python 
In [9]: a
Out[9]: 
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])

In [10]: np.sum(a, axis=0)
Out[10]: array([12, 15, 18])

In [11]: np.sum(a, axis=1)
Out[11]: array([ 6, 15, 24])
```


np.sum 的 keepdims 参数  
```python 
In [17]: a
Out[17]: 
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])

In [18]: np.sum(a, axis=0, keepdims=True)
Out[18]: array([[12, 15, 18]])

In [19]: np.sum(a, axis=1, keepdims=True)
Out[19]: 
array([[ 6],
       [15],
       [24]])
```


算指数  
```python 
x = np.array([1, 2, 3, 4])  
np.exp(x)  
```


读取 .npy 文件  
```python 
import numpy as np 

file = np.load('test_logits.npy') 
print(file) 
np.savetxt('test_logits.txt', file)  
```

或者  
```python 
import numpy as np 

test_logit = np.load('clue_classifier/model/single_update/bert/test_logits.npy')  
test_logit.shape  
test_logit[:10]  
```


