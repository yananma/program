

### 常用命令  

```python 
df.head()  

df.head(10)  

# 取一行  
df.iterrows()
```


### read_csv()  

```python
df = pd.read_csv('ocr.csv')   # 直接传文件名就可以  

df = pd.read_csv(str(settings.RESOURCE_ROOT / 'docs' / 'program' / 'pika.csv'))

# 不添加表头  
df = pd.read_csv('ocr.csv', header=None)  

# 可以传链接    
df = pd.read_csv('http')
```

### read_excel()  

```python 
df = pd.read_excel('模型反馈样本数据.xlsx', engine='openpyxl', sheet_name='c_label0_some_preds1')
```

[read_excel 文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)    


### to_excel()

写入多个 sheet  

```python 
df1 = pd.DataFrame(result_list)
df2 = pd.DataFrame(result_list[:5])

with pd.ExcelWriter('output.xlsx') as writer:  
    df1.to_excel(writer, sheet_name='Sheet_name_1')
    df2.to_excel(writer, sheet_name='Sheet_name_2')
```

[to_excel 文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)  


### pandas 排序  

```python 
df = df.sort_values(by=列名，没有列名的用 0 1 2 3 等这些数字)
```


### 判断非 nan  

```python 
if not pd.isna(line[1]['names'])
```


### 消除 url 类型数量限制  

```python 
with pd.ExcelWriter(f'{work_root}/{suffix}.xlsx',engine='xlsxwriter',options={'strings_to_urls': False}) as writer:
    newdf.to_excel(writer)
```


### 高亮  

write_rich_string 某个词高亮  
```python 
from xlsxwriter.workbook import Workbook

workbook = Workbook(r'test.xlsx')  # 创建xlsx
worksheet = workbook.add_worksheet('A')  # 添加sheet
red = workbook.add_format({'color': 'red'})  # 颜色对象
worksheet.write(0, 0, 'sentences')  # 0，0表示row，column，sentences表示要写入的字符串
test_list = ["我爱", "中国", "天安门"]
test_list.insert(1, red)  # 将颜色对象放入需要设置颜色的词语前面，这里是放在中国前面
print(test_list)
worksheet.write_rich_string(1, 0, *test_list)  # 写入工作簿
workbook.close()  # 记得关闭
```

write_string 整句高亮  
```python 
from xlsxwriter.workbook import Workbook

workbook = Workbook(r'test.xlsx')  # 创建xlsx
worksheet = workbook.add_worksheet('A')  # 添加sheet
red = workbook.add_format({'color': 'red'})  # 颜色对象
worksheet.write(0, 0, 'sentences')  # 0，0表示row，column，sentences表示要写入的字符串
test_list = ["我爱中国天安门"]
test_list.insert(1, red)  # 将颜色对象放入需要设置颜色的词语后面，这里是放在句子后面
print(test_list)
worksheet.write_string(1, 0, *test_list)  # 写入工作簿
workbook.close()  # 记得关闭
```



### 其他

```python 
import pandas as pd

df = pd.DataFrame({'Names':['Andreas', 'George', 'Steve',
                           'Sarah', 'Joanna', 'Hanna'],
                  'Age':[21, 22, 20, 19, 18, 23]})
```

不单单是字典套列表，pandas 非常强大，可以列表套列表。  

```python 
import logging

from LAC import LAC
from django.conf import settings
from django.core.management.base import BaseCommand
import pandas as pd


class Command(BaseCommand):
    def add_arguments(self, parser):
        self.lac = LAC(mode='lac')
        self.lac.load_customization(str(settings.RESOURCE_ROOT / 'docs' / 'program' / 'lac_person_costom.txt'))
        pass

    def handle(self, *args, **options):
        pmap = {
            'PER': '人名',
            'LOC': '地名',
            'ORG': '机构名',
            'TIME': '时间',
        }
        li = []
        for line in (settings.RESOURCE_ROOT / 'docs' / 'program' / '姓名测试新的例子.txt').read_text(encoding='utf8').splitlines():
            line = line.strip()
            if not line:
                continue
            ws, ps = self.lac.run(line)
            li.append(ws)
            li.append(ps)
            li.append([pmap[p] for p in ps])
            li.append('\n')

        dataframe = pd.DataFrame(li)
        dataframe.to_csv(str(settings.RESOURCE_ROOT / 'docs' / 'program' / 'test_name.csv'), index=False, sep=',')
        print('写入完毕。。。。。。')
```

```python 
# 显示所有行  
pd.set_option('display.max_rows', None)  

# 设置行宽  
pd.set_option('max_colwidth',200)
```


[[]] 第一个 [] 表示所有列名，第二个 [] 中可以传入多个列  

apply，已知函数无法实现功能，自定义函数。  

是对 apply 的 . 之前的数据，进行括号 :后面的操作  

groupby()[]： ()里是谁，列就是谁，[] 是这个谁的某些属性.  

concat 是拼接，两块玻璃，侧面粘胶水    
merge 是合并，如果相同就共同使用，有重叠  


使用 Timestamp 以后，就可以调用 year、month、day 等各种属性了

replace=True，替换  
replace=False，新建  


