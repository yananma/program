
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


