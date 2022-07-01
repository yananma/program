
### 文档就是最好的笔记，多读文档


要用 notebook 处理 pandas，永远不要用 pycharm 处理 pandas，pycharm 打印的结果看不到结构，会耽误很多时间。    


### 遍历行   

```python 
for i, row in df.iterrows():
    asr = row['语音']
```

要注意的是，遍历迭代的时候，不会改变原来的 DataFrame.   

```python
def label_data():
    df = pd.read_excel('green_data.xlsx')
    df.fillna('', inplace=True)
    tag_list = get_tag_list()
    for i, row in df.iterrows():
        yuyin, zimu, huazi = row['语音'], row['字幕'], row['花字']
        for tag in tag_list:
            key = str(list(tag.keys())[0])
            value = str(list(tag.values())[0])
            if value in yuyin or value in zimu or value in huazi:
                row[key] = 1  # 1表示有该标签
            else:
                row[key] = 0  # 0表示没有该标签
    return df
```

上面这种写法最后返回的还是第一行读入的结果，赋值语句没有发挥作用。不可变。    

正确写法   

```python
def label_data():
    df = pd.read_excel('green_data.xlsx')
    df.fillna('', inplace=True)
    tag_list = get_tag_list()
    new_df = pd.DataFrame(columns=df.columns)
    for i in range(len(df)):
        row = df.iloc[i]
        new_row = {}
        # new_row = row.copy()  # 如果是只更新几个字段，其他的保持不变，就用 .copy()    
        yuyin, zimu, huazi = row['语音'], row['字幕'], row['花字']
        for tag in tag_list:
            key = str(list(tag.keys())[0])
            value = str(list(tag.values())[0])
            if value in yuyin or value in zimu or value in huazi:
                new_row[key] = 1  
            else:
                new_row[key] = 0  
        new_df = pd.concat([new_df, pd.DataFrame(new_row, index=[i])], ignore_index=True)
        # new_df = new_df.append(new_row, ignore_index=True)  # 低版本 pandas 要用 append，多低不知道，至少 0.24.2 是这样。    
    return new_df
```

iterrows 官方文档：You should never modify something you are iterating over. This is not guaranteed to work in all cases. Depending on the data types, the iterator returns a copy and not a view, and writing to it will have no effect.   


### 取某一列的数据   

```python 
# df['列名']
df['原三级标签']   
```


### 删除列   

要指定 axis=1   

```python 
df.drop(['二级标签'], axis=1)   
```

删除多列     

```python 
df = df.drop(['二级标签', '提及数量', 'Unnamed: 5'], axis=1)
```    

切片，从第 10 列开始往后取    
```python 
origin_df = origin_df.drop(origin_df.columns[:10], axis=1)     
```


### [rename](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rename.html)   

```python 
df = df.rename(columns={'Unnamed: 1': '二级标题'})
```


### [某一列设为索引](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.set_index.html#pandas.DataFrame.set_index)   

```python 
df = df.set_index('二级标题')
```

多列设为索引   

```python 
df = df.set_index(['二级标题', '拆解/合并三级标签项建议'])
```


### 设置完索引以后取索引值  

```python 
df = df.set_index(['二级标题', '拆解/合并三级标签项建议'])

df.loc['Brand（品牌）'].loc['L\'Oreal']   
```   


### [pd.concat 拼接](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html)

```python 
pd.concat([s1, s2], ignore_index=True)
0    a
1    b
2    c
3    d
dtype: object
```

### [df.dropna](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.dropna.html)

这三列，所有的都为 nan 才删除。   

```python 
df = pd.read_excel('green_data.xlsx')
df = df.dropna(subset=['语音', '字幕', '花字'], how='all')
```


### 取 CSV 的表头   

```python 
df.columns

# 取值按索引取
df.columns[0]  
```

用 columns.values 也可以   

```python 
# df.columns 结果为 Index 类型，.values 结果为列表  
df_tag.columns.values   
```


### [read_csv()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html)  

```python
# 用 f 字符串拼最好   
df = pd.read_csv(f'{self.file_dir}{self.name}.csv')   

df = pd.read_csv('ocr.csv')   # 直接传文件名就可以  

df = pd.read_csv(str(settings.RESOURCE_ROOT / 'docs' / 'program' / 'pika.csv'))

# 不添加表头  
df = pd.read_csv('ocr.csv', header=None)  
```


### [read_excel()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)      

```python 
df = pd.read_excel('模型反馈样本数据.xlsx', engine='openpyxl', sheet_name='c_label0_some_preds1')
```

读取多个 sheet，指定 sheet_name=None，返回的 df 是一个字典，键是 sheet 名称   

```python 
df = pd.read_excel('./data/车型车系表V6.xlsx', sheet_name=None)
brand_list = list(df.keys())
```


第一行不是 header，指定参数 header=None   

拼接文件路径  

```python 
UPLOADS_DIR = '/home/test/syb/mayanan/reci/resources/uploads/'

df = pd.read_excel(UPLOADS_DIR + '跑热词.xlsx', engine='openpyxl')
```

以第二行为表头   

```python 
df = pd.read_excel('护发素视频0-1数据(1)_语音_OCR.xlsx', header=1)
```


### 把列表写入到 Excel  

```python 
result_list = []  
for item in data_list:  
    result_dict = {}  
    result_dict["title"] = item["title"] 
    result_list.append(result_dict)  
    
df = pd.DataFrame(result_list)  
df.to_csv(file_name, index=False)  
```


### to_excel()

写入多个 sheet  

```python 
df1 = pd.DataFrame(result_list)
df2 = pd.DataFrame(result_list[:5])

with pd.ExcelWriter('output.xlsx') as writer:  
    df1.to_excel(writer, sheet_name='Sheet_name_1')
    df2.to_excel(writer, sheet_name='Sheet_name_2')
```


#### 追加文件  

指定参数 mode='a'，而且第一个写入的时候有表头，后面不要写表头，指定参数 headers=False   

```python 
exist_flag = False   # 这个定义写在 for 循环外面  
if not exist_flag:
    df.to_csv("/home/test/syb/mayanan/zjgdk/resources/专家观点库微博0613.csv", index=False, mode="a")
    exist_flag = True
else:
    df.to_csv("/home/test/syb/mayanan/zjgdk/resources/专家观点库微博0613.csv", header=False, index=False, mode="a")
``` 


[to_excel 文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)  


### pandas 排序  

```python 
df = df.sort_values(by=列名，没有列名的用 0 1 2 3 等这些数字)

df = df.sort_values(by=[0, 3], key=lambda x: x if x.name == 0 else x.map(lambda x: json.loads(x)[0][1]))  # 先按第 0 列名称排序，再按第 3 列位置排序，位置要做处理
```


### 统计 nan 的数量  

```python 
df_ads.isna().sum()
``` 


### 判断非 nan  

```python 
if not pd.isna(line[1]['names'])
```


### [fillna](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.fillna.html)   


如果内容为 nan，就没有办法用 in 判断是否包含，会报错。所以解决办法就是 nan 替换成空字符串。    

```python 
fillna(value='')  
```


下面这种不再使用   

```python 
line['title'] if not pd.isna(line['title']) else ''
```

函数    
```python 
def replace_nan(self):
    """把字典中的 nan 替换为空"""
    for key, val in self.post.items():
        if pd.isna(val):
            self.post[key] = ''
```


### 删除含有 nan 的行  

```python 
df.dropna(axis=0, how='any')  
```


### 替换 nan 为空字符串  

```python 
def replace_nan_with_blank(row):
    for key in row.keys():
        if pd.isnull(row[key]):
            row[key] = ""


for i, row in df.iterrows():
    replace_nan_with_blank(row)  
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


### openpyxl   

```python 
            baseDir = settings.BASE_DIR
            new_upload_file_name = baseDir + "/static/changcheng_datas/changcheng_report.xlsx"
            with open(new_upload_file_name, 'wb') as f:
                f.write(fname3.read())
            wb = openpyxl.load_workbook(new_upload_file_name)
            sheet_content = wb[u'长城汽车负面汇总']
            rows = sheet_content.max_row
            for i in range(2, rows + 1):
                qudao = sheet_content.cell(row=i, column=9).value or ''
                pinpai = sheet_content.cell(row=i, column=17).value or ''
                chexi = sheet_content.cell(row=i, column=18).value or ''
                chexing = sheet_content.cell(row=i, column=19).value or ''
                key = pinpai + "$" + chexi + "$" + chexing
                sheet_content.cell(1, 29, u'舆情处理')
                sheet_content.cell(1, 30, u'舆情感知')
                if qudao in [u'新闻', u'视频', u'微博', u'微信'] and key in qudao1_rules:
                    yuqingchuli, yuqingganzhi = qudao1_rules[key].split('$')
                    sheet_content.cell(row=i, column=29).value = yuqingchuli
                    sheet_content.cell(row=i, column=30).value = yuqingganzhi
                elif qudao in [u'论坛', u'问答', u'贴吧', u'小红书', u'懂车帝车友圈'] and key in qudao2_rules:
                    yuqingchuli, yuqingganzhi = qudao2_rules[key].split('$')
                    sheet_content.cell(row=i, column=29).value = yuqingchuli
                    sheet_content.cell(row=i, column=30).value = yuqingganzhi

        wb.save(new_file_name)
        file = open(new_file_name, "rb")
        response = FileResponse(file)
        response['Content-Type'] = 'application/octet-stream'
        response['Content-Disposition'] = 'attachment;filename="{}"'.format("changcheng_new.xlsx")
        return response
``` 


## 代码   

#### 简单处理  

```python 
#!/usr/bin/env python
# coding: utf-8

import pandas as pd 

green_col_list = ['Brand（品牌）', 'Product Benefit（产品功效）', 'Ingredient（成分）', 'Texture（质地）', 'Scent（香味）',
                  'Platform（平台）', 'Subordinate Campaign（话题）', 'In-wash Experience（使用体验感）', 'Tension Point/痛点',
                  'Seasonal Message（春夏秋冬、节日）', 'Promotion（大促活动）',
                  'Call for action（是否推动消费者下单，附带购买链接）', '发质（中性、干性、油性、酸性、碱性）']

df = pd.read_excel('./sample-1.xlsx', sheet_name='biaoqianshu')

df = df.drop(['二级标签', '提及数量', 'Unnamed: 5'], axis=1)

df = df.rename(columns={'Unnamed: 1': '二级标题'})

df = df.fillna(method='ffill')

df = df.set_index(['二级标题'])

df_list = []
for col in green_col_list:
    df_list.append(df.loc[col])

df = pd.concat(df_list, ignore_index=True)

df.to_excel('green_tag.xlsx', index=False)
```


#### dropna  

```python 
df = pd.read_excel('green_data.xlsx')
df = df.dropna(subset=['语音', '字幕', '花字'], how='all')
```

等同于   

```python 
def data_remove_nan():
    df = pd.read_excel('green_data.xlsx')
    df.fillna('', inplace=True)
    new_df = pd.DataFrame(columns=df.columns)
    for i in range(len(df)):
        row = df.iloc[i]
        new_row = {}
        yuyin, zimu, huazi = row['语音'], row['字幕'], row['花字']
        if any([yuyin, zimu, huazi]):
            for key in df.columns:
                new_row[key] = row[key]
            new_df = pd.concat([new_df, pd.DataFrame(new_row, index=[i])], ignore_index=True)
    return new_df
```


## 报错   

### XLRDError: Excel xlsx file； not supported 报错

原因是pip安装的是最新的 2.0.1 版本，只支持 .xls 文件。所以 pandas.read_excel(‘xxx.xlsx’) 会报错。   

可以安装旧版xlrd，在 cmd 中执行指令：    
```python
pip install xlrd==1.2.0
```


### 'ascii' codec can't decode byte 0xe5 in position 0: ordinal not in range(128)  python2.7 pandas0.24.2  

要指定 engine 和 encoding   
```python 
new_df.to_excel(excel_name, engine='xlsxwriter', index=False, encoding='utf-8')
``` 



