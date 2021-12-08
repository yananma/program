

```python 
import numpy as np 

x = np.array([1, 2, 3, 4]) 
y = np.array([5, 6, 7, 8]) 
a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])  
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


