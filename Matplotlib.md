
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


#### 画多个图  

```python 
import matplotlib
import matplotlib.pyplot as plt

excel_default_palette_b8 = [
    (0, 0, 0), (255, 255, 255), (255, 0, 0), (0, 255, 0),
    (0, 0, 255), (255, 255, 0), (255, 0, 255), (0, 255, 255),
    (128, 0, 0), (0, 128, 0), (0, 0, 128), (128, 128, 0),
    (128, 0, 128), (0, 128, 128), (192, 192, 192), (128, 128, 128),
    (153, 153, 255), (153, 51, 102), (255, 255, 204), (204, 255, 255),
    (102, 0, 102), (255, 128, 128), (0, 102, 204), (204, 204, 255),
    (0, 0, 128), (255, 0, 255), (255, 255, 0), (0, 255, 255),
    (128, 0, 128), (128, 0, 0), (0, 128, 128), (0, 0, 255),
    (0, 204, 255), (204, 255, 255), (204, 255, 204), (255, 255, 153),
    (153, 204, 255), (255, 153, 204), (204, 153, 255), (255, 204, 153),
    (51, 102, 255), (51, 204, 204), (153, 204, 0), (255, 204, 0),
    (255, 153, 0), (255, 102, 0), (102, 102, 153), (150, 150, 150),
    (0, 51, 102), (51, 153, 102), (0, 51, 0), (51, 51, 0),
    (153, 51, 0), (153, 51, 102), (51, 51, 153), (51, 51, 51),
]


for i, color in enumerate(excel_default_palette_b8):
    color = tuple(map(lambda x: x / 255, color))
    plt.subplot(14, 4, i + 1, facecolor=color)  # 背景色设置，参数是 行、列、位置索引   
    print(i + 1, color)
plt.show()
```  


#### 显示中文  
`plt.rcParams['font.sans-serif']=['SimHei']`   


#### 柱状图标签显示中文   

```python
ax = plt.gca(); 
ax.xaxis.set_ticklabels(top_six, fontproperties=FontProperties(fname=font_name, size=12))   
```    


