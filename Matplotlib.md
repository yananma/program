
#### 最基本的画图  

```python 
import matplotlib.pyplot as plt 

x = [item for item in range(10)]  
y = [item ** 2 for item in range(10)]

plt.plot(x, y)   
```


#### 画多条线  

```python 
import matplotlib.pyplot as plt

x = [x for x in range(10)]
y1 = [x ** 2 for x in range(10)]
y2 = [x ** 3 for x in range(10)]

# 方法 1 
plt.plot(x, y1, y2)

# 方法 2
plt.plot(x, y1)
plt.plot(x, y2)
```


#### 显示中文  
`plt.rcParams['font.sans-serif']=['SimHei']`   
