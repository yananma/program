
### 创建向量和矩阵  
```python 
import numpy as np 

x = np.array([1, 2, 3, 4]) 
y = np.array([5, 6, 7, 8]) 
a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])  
```


### np.sort 
```python 
In [4]: import numpy as np

In [5]: x = np.array([6, 3, 9, 2])

In [6]: np.sort(x)
Out[6]: array([2, 3, 6, 9])
```


### axis；0 纵向，1 横向    
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


### np.sum 的 keepdims 参数  
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


### 算指数  
```python 
x = np.array([1, 2, 3, 4])  
np.exp(x)  
```


### softmax 函数  
```python 
In [34]: a
Out[34]: 
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])

In [35]: def softmax(X):
    ...:     """softmax 函数"""
    ...:     X_exp = np.exp(X)
    ...:     partition = np.sum(X_exp, axis=1, keepdims=True)
    ...:     return X_exp / partition
    ...: 

In [36]: softmax(a)
Out[36]: 
array([[0.09003057, 0.24472847, 0.66524096],
       [0.09003057, 0.24472847, 0.66524096],
       [0.09003057, 0.24472847, 0.66524096]])
```


### 读取 .npy 文件  
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


