
读取 .npy 文件  

```python 
import numpy as np 

file = np.load('test_logits.npy') 
print(file) 
np.savetxt('test_logits.txt', file)  
```
