
学习 pandas 的方法就是自己构造数据，多试。看每一步的变化，看每一步的结果。    

问文心一言    
搜百度，看 20 篇文章    
读源码     
读文档    


多用 ? 看用法，看例子，多敲代码。    
dir、dir(collections)、dir(requests)


不管结果怎么样，都要先自己试一试再说。    

试完之后问有没有更好的办法。   



# 读书    

- [ ] 第一遍照着敲
- [ ] 复习一遍
- [ ] 修改，实验    


### 第五章   

Series 可以当成一个字典。    








---   


```python
In [2]: datas = [{u"name": u"a", u"type": -1, u"count": 200}, 
   ...:          {u"name": u"a", u"type": 0, u"count": 300},
   ...:          {u"name": u"a", u"type": 1, u"count": 400},
   ...:          {u"name": u"b", u"type": -1, u"count": 500},
   ...:          {u"name": u"b", u"type": 0, u"count": 600},
   ...:          {u"name": u"b", u"type": 1, u"count": 700}]

In [3]: df = pd.DataFrame(datas)

In [4]: df
Out[4]: 
   count name  type
0    200    a    -1
1    300    a     0
2    400    a     1
3    500    b    -1
4    600    b     0
5    700    b     1
```


---  


对于复杂的写入格式，比如表头、排序、样式，比如复杂的表格，不用 pandas，因为 pandas 自己不熟，操作起来很复杂；改成使用 python 操作，通过原生的方法写入。csv 使用 csv，excel 使用 xlsxwriter。自己写 pgc 导出，写了七八个小时，完成得还不好，如果用 python 操作可能半个小时就写完了。      

如果要写入样式，就去 WPS 里看样式，比如字体字号，然后去问 ChatGPT，这些该怎么实现就可以了。   

---   


结构不好的文档，全部按照官方文档的结构重新调整

# Input/output  

### [read_csv()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html)  

```python
# 用 f 字符串拼最好   
df = pd.read_csv(f'{self.file_dir}{self.name}.csv')   

df = pd.read_csv('ocr.csv')   # 直接传文件名就可以  

df = pd.read_csv(str(settings.RESOURCE_ROOT / 'docs' / 'program' / 'pika.csv'))

# 不添加表头  
df = pd.read_csv('ocr.csv', header=None)  
```

python2 要指定 encoding，否则，还要遍历每一列，一行一行 decode    

```python 
df = pd.read_csv(file_path, encoding=u"utf-8")
```

否则就会像下面这样    

```python 
# -*- coding: utf-8 -*-
import traceback

import pandas as pd
from django.conf import settings
from django.core.management.base import BaseCommand
from pathlib import Path

from xposts.models import Xpost
from utils.common import get_logger

logger = get_logger(u"tongyong_update_db_sentiment")


class Command(BaseCommand):
    help = u"读取 CSV 文件，更新数据库的正负面分数字段"

    def handle(self, *args, **options):
        file_path = Path(settings.BASE_DIR) / u"temp" / u"sentiment" / u"test_comment_10011015_aodi_predict.csv"
        df = pd.read_csv(file_path)
        obj_list = []
        field_list = [u"type_rank", u"pos_type_rank"]
        for row in df.to_dict(u"records"):
            row = {key.decode(u"GB2312"): value for key, value in row.items()}  # 可以用 EmEditor 打开文件，看文件编码。
            p = Xpost.objects.only(u"postid", u"type_rank", u"pos_type_rank").get(postid=row[u"postid"])
            p.type_rank = int(row[u"负面分数"] * 100)
            p.pos_type_rank = int(row[u"正面分数"] * 100)
            obj_list.append(p)
        Xpost.objects.bulk_update(obj_list, field_list, batch_size=5000)
        logger.info(u"finish.")
```


### to_csv()  

存 CSV 用 EmEditor 比用 pandas 快多了。    

用 navicat 导出 csv 格式的数据的时候，用 WPS 打开是乱码，然后用 pandas 读到内存里，指定 encoding='utf-8'，看到已经不是乱码了。to_csv 的时候，指定 encoding='gbk' 打开还是乱码，就再加一个参数 errors='ignore' 就可以了.     

`pd.concat([df1, df2, df3, df4, df5, df6]).to_csv('dazhongchezhan.csv', encoding='gbk', errors='ignore')`    


#### 'ascii' codec can't decode byte 0xe5 in position 0: ordinal not in range(128)

```python
# 加 encoding 参数
df.to_csv('beijingchezhan.csv', index=False, encoding='utf-8')
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


### [read_excel()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)      

df 切片，测试的时候只取几条用，df[0:3]

```python 
df = pd.read_excel('模型反馈样本数据.xlsx', engine='openpyxl', sheet_name='c_label0_some_preds1')
```

读取多个 sheet，指定 sheet_name=None，返回的 df 是一个字典，键是 sheet 名称，值是 DataFrame       

```python 
df = pd.read_excel('./data/车型车系表V6.xlsx', sheet_name=None)
brand_list = list(df.keys())
for brand in brand_list:
    brand_df = df[brand]    
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


### 读 url 链接   

```python
def read_authors(self):
    url = (u"https://oss.static.yuqingsee.com/staticfiles/cyberin_b33/export_temp_files/sp/%E5%A4%A7%E4%BC%97%E5%"
           u"93%94%E5%93%A9%E5%93%94%E5%93%A9.xlsx")
    df = pd.read_excel(url)
    return df['author'].tolist()
```

### to_excel()

写入的时候，指定列的顺序   

```python 
result_df = pd.DataFrame(result_list)（注意是两层列表）[[u'时间', u'编辑库', u'发布库', u'捡漏池', u'总量']] 
```  


写入多个 sheet  

```python 
df1 = pd.DataFrame(result_list)
df2 = pd.DataFrame(result_list[:5])

with pd.ExcelWriter('output.xlsx', engine='xlsxwriter', options={'strings_to_urls': False}) as writer:  
    df1.to_excel(writer, sheet_name='Sheet_name_1')
    df2.to_excel(writer, sheet_name='Sheet_name_2')
```

函数   

```python  
def write_to_excel(self, sheet_datas):
    filepath = u'通用MKT品牌周报大数需求_{}.xlsx'.format(datetime.datetime.now().date())
    writer = pd.ExcelWriter(filepath, engine='xlsxwriter', options={'strings_to_urls': False})
    for sheet_name, sheet_data in sheet_datas.items():
        pd.DataFrame(sheet_data).to_excel(writer, sheet_name, index=False, header=False)
    writer.close()
    return filepath
```


[to_excel 文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html)  



# Series   

Series 是一维的数据，当做列表。    


### 把一列转成列表 to_list    

```python
df = pd.read_excel(BytesIO(requests.get(url).content), encoding='utf-8', sheet_name=u'负面词', header=None).fillna('')
df['性别'].to_list()
```


### 列重命名 [rename](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rename.html)   

```python 
df = df.rename(columns={'Unnamed: 1': '二级标题'})
```


### 删除含有 nan 的行 dropna  

```python 
df.dropna(axis=0, how='any')  
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

下面这种也不再使用   

函数    
```python 
def replace_nan(self):
    """把字典中的 nan 替换为空"""
    for key, val in self.post.items():
        if pd.isna(val):
            self.post[key] = ''
```



### 判断非 nan， isna  

不要用 if 判断，if nan 结果是 True    

```python 
if not pd.isna(line[1]['names'])
```


### 统计 nan 的数量  

```python 
df_ads.isna().sum()
``` 




### group by 取 top value_count   

```python
In [1]: import pandas as pd

In [2]: url = u"https://oss.static.yuqingsee.com/staticfiles/cyberin/export_temp_files/sp/top_location.csv"

In [3]: df = pd.read_csv(url)

In [5]: df.shape
Out[5]: (7892797, 2)

In [7]: df["author_location"].value_counts()
```



# DataFrame  



### DataFrame  

可以通过 index 指定行，指定显示的行，指定行的顺序。   

可以通过 columns 指定列，指定显示的列，指定列的顺序。   



### 查看行数 shape   

```python
df.shape    
```



### 遍历行 iterrows  

数据量大的时候，df.iterrows() 会非常慢，要转成 dict，转成 Python 对象。     

```python 
sentiment_df = pd.read_excel("./data/通用项目月度数据.xlsx", sheet_name="Sheet1").to_dict("records")

for row in sentiment_df:
    print(row)
``` 

python2 要指定 encoding 参数，否则很麻烦。   

```python 
sentiment_df = pd.read_excel("./data/通用项目月度数据.xlsx", sheet_name="Sheet1", encoding='utf-8').to_dict("records")    
```


最好是只转换一次，也就是在最开始的地方转成 dict。   


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



### 读某一列的数据（先切片再读，占内存低）   

因为 Python2 源码里有把列名 encode 成 str，但是返回的又是 Unicode，导致报错 ValueError      

usecols 可以传函数。   

```python 
def usecols(x):
    return x in {u"监控对象", u"摘要", u'主题', u'文章类型'}


pd.read_csv(file_obj, encoding='gbk', usecols=usecols).fillna(u'')   
```


### 取某一列的数据（读完再切片，占内存和读全部一样）     

```python 
# df['列名']
df['原三级标签']   
```

### 取某一行   

loc、iloc   

```python
df[df['A'] == 2]

df[df["type"].isin([1, -1])]  
```


### 删除列   

1 是竖着的，1 的形状是纵向的，就是列。    

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


### [df.dropna](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.dropna.html)

这三列，所有的都为 nan 才删除。   

```python 
df = pd.read_excel('green_data.xlsx')
df = df.dropna(subset=['语音', '字幕', '花字'], how='all')
```



### [某一列设为索引](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.set_index.html#pandas.DataFrame.set_index)   

写入 excel 的时候，指定索引列   

```python 
df.set_index(u'时间').to_excel(file_name, sheet_name=sheet_name)   
```


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
s1 = pd.Series(['a', 'b'])  # Series 是一列    
s2 = pd.Series(['c', 'd'])
pd.concat([s1, s2], ignore_index=True)
0    a
1    b
2    c
3    d
dtype: object
```

可以把多个 df append 到一个 list 中，然后拼接最后的 list     

```python 
def concat_excel():
    df_list = []
    for file in os.listdir('./data/san_ji_label/result/'):
        df = pd.read_excel(f"./data/san_ji_label/result/{file}")
        df_list.append(df)
    result_df = pd.concat(df_list)
    print(f"len result_df：{len(result_df)}")
    result_df.to_excel(f"./data/san_ji_label/result/concat_result.xlsx", index=False)
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




### 把列表写入到 Excel  

```python 
In [7]: import pandas as pd 

In [8]: li = [{'a': 1, 'b': 2}, {'a': 3, 'b': 4}, {'a': 5, 'b': 6}, {'a': 7, 'b': 8}]

In [9]: pd.DataFrame(li)
Out[9]: 
   a  b
0  1  2
1  3  4
2  5  6
3  7  8
```

```python 
result_list = []  
for item in data_list:  
    result_dict = {}  
    result_dict["title"] = item["title"] 
    result_list.append(result_dict)  
    
df = pd.DataFrame(result_list)  
df.to_csv(file_name, index=False)  
```



### pandas 排序  

```python 
df = df.sort_values(by=列名，没有列名的用 0 1 2 3 等这些数字)

# 先按第 0 列名称排序，再按第 3 列位置排序，位置要做处理
df = df.sort_values(by=[0, 3], key=lambda x: x if x.name == 0 else x.map(lambda x: json.loads(x)[0][1]))  
```

```python 
# df 倒序      

df.sort_index(ascending=False)
```



### 消除 url 类型数量限制  

pandas，pandas 不能在 to_excel 里写 options 会报错没有这个参数，必须要用下面这种写法         

```python 
with pd.ExcelWriter(f'{work_root}/{suffix}.xlsx', engine='xlsxwriter', options={'strings_to_urls': False}) as writer:
    newdf.to_excel(writer)
```

xlsxwriter      

```python 
xlsxwriter.Workbook(filename, {'strings_to_urls': False})     
```

高版本 pandas   

```python
with pd.ExcelWriter('result.xlsx', engine='xlsxwriter', engine_kwargs={"options": {'strings_to_urls': False}}) as writer:
    df.to_excel(writer, index=False)
```


### 列转为字典  

```python
import pandas as pd

df = pd.read_clipboard()

keywords = df["关键词"].unique()

d = {}
for keyword in keywords:
    print(keyword)
    print(df[df["关键词"] == keyword])
    d[keyword] = df[df["关键词"] == keyword]["fid"].tolist()

print(d)
```


### 抽样脚本  

```python
import pandas as pd


df = pd.read_csv("../data/tongyong/揽胜2023正面评论.csv", encoding='gbk', index_col=False)

# 抽样 50 万条
df = df.sample(n=500000)
df.to_csv("../data/tongyong/揽胜2023正面评论_50w.csv", index=False)
``` 


### 读取错位问题  

默认以第一列为 index，如果不需要第一列为 index 要加 index_col=False 参数。      




### 把数量转变成百分比   

```python
import pandas as pd

# 假设你的DataFrame数据如下
data = {
    'name': ['大众', '大众', '大众', '通用', '通用', '通用'],
    'sentiment': ['正', '中', '负', '正', '中', '负'],
    'count': [10, 20, 30, 40, 50, 60]
}

df = pd.DataFrame(data)

# 利用groupby根据'name'和'sentiment'分组，并计算每组的'count'总和，然后重置索引
grouped_sum = df.groupby(['name'])['count'].sum().reset_index(name='total_count')

# 将原始DataFrame与分组总和进行合并
df = pd.merge(df, grouped_sum, on=['name'])

# 计算百分比
df['percentage'] = (df['count'] / df['total_count']) * 100

# 若只需要name、sentiment和percentage列，可选择这些列展示
result_df = df[['name', 'sentiment', 'percentage']]

# 打印处理后的DataFrame
print(result_df)

```

然而，上述方法稍显繁琐，因为我们进行了分组、求和、合并以及百分比计算等多个步骤。实际上，我们可以利用groupby和transform函数来更简洁地实现这一目标：   

```python
# 利用transform函数计算每组的总和，并直接用于百分比计算
df['total_count'] = df.groupby(['name'])['count'].transform('sum')
df['percentage'] = (df['count'] / df['total_count']) * 100

# 选择所需的列进行展示
result_df = df[['name', 'sentiment', 'percentage']]

# 打印结果
print(result_df)

```

在这两种方法中，第二种方法更为简洁且高效。最后，如果你希望百分比保留特定的小数位数，可以利用round函数：   

```python
result_df['percentage'] = result_df['percentage'].round(2) # 保留两位小数
print(result_df)
```

转换成百分比：   

```python
df["percentage"] = df["percentage"].map(lambda x: format(x, ".2%"))
```


### 判断数字是否连续  

```python
def find_discontinuous_numbers(lst):  
    # 假设列表中的数字是唯一的，并且我们想要找出不连续的区间  
    if not lst:  # 空列表没有不连续的数字  
        return []  
  
    # 对列表进行排序  
    lst.sort()  
  
    # 初始化上一个数字为列表的第一个元素  
    prev_num = lst[0]  
    discontinuous_intervals = []  # 存储不连续区间的列表  
  
    # 遍历列表（从第二个元素开始）  
    for num in lst[1:]:  
        # 检查当前数字与前一个数字的差值  
        diff = num - prev_num  
        # 如果差值大于1，说明有不连续的数字  
        if diff > 1:  
            # 记录不连续区间（可以使用元组(prev_num + 1, num - 1)来表示区间，  
            # 但这里为了简单起见，我们只记录开始和结束的数字）  
            discontinuous_intervals.append((prev_num + 1, num - 1))  
  
        # 更新前一个数字为当前数字  
        prev_num = num  
  
    # 如果列表的最后一个元素与倒数第二个元素的差值大于1，  
    # 那么最后一个元素后面也有一个不连续区间（例如，列表[1, 2, 4]）  
    if lst[-1] - lst[-2] > 1:  
        discontinuous_intervals.append((lst[-2] + 1, lst[-1] - 1))  
  
    return discontinuous_intervals  
  
# 示例  
numbers = [1, 2, 4, 5, 7, 8, 10]  
print(find_discontinuous_numbers(numbers))  # 输出: [(3, 3), (6, 6), (9, 9)]
```


### 判断时间序列是不是连续的   

```python
import pandas as pd
# 示例时间数据
timestamps = ['2022-01-01', '2022-01-02', '2022-01-04', '2022-01-05']
# 将时间数据转换为DatetimeIndex对象
dt_index = pd.to_datetime(timestamps)
# 判断时间是否连续
is_continuous = pd.infer_freq(dt_index) is not None
if is_continuous:
    print("时间是连续的")
else:
    print("时间不是连续的")
```


### 找到时间中断的地方    

```python
import pandas as pd
# 示例时间数据
timestamps = ['2022-01-01', '2022-01-02', '2022-01-04', '2022-01-05']
# 将时间数据转换为DatetimeIndex对象
dt_index = pd.to_datetime(b)
# 计算时间间隔
time_diff = dt_index.to_series().diff()
# # 找出中断点的索引
interrupted_indexes = time_diff[time_diff > pd.Timedelta(days=1)].index
# # 输出中断点
for index in interrupted_indexes:
    print(index-pd.Timedelta(days=1))
```


# XlsxWriter   

文档 `https://xlsxwriter.readthedocs.io/`    


xlsxwriter 日期格式  

`date_format = workbook.add_format({'num_format': 'yyyy-mm-dd hh:mm:ss', 'align': 'center'})`   



```python
workbook = xlsxwriter.Workbook(self.filepath)
worksheet = workbook.add_worksheet(u"美团政策类信息汇总")
worksheet.set_column(0, 12, 12)
center_format = workbook.add_format({'align': 'center'})
blue_format = workbook.add_format({'color': 'blue', 'underline': 1, 'align': 'center'})
worksheet.merge_range(0, 0, 0, 12, u"美团安全政策类信息汇总{}-{}".format(self.format_date(self.args[u"date1"]), self.format_date(self.args[u"date2"])), center_format)
head_row = [u"序号", u"专业方向", u"提及类型", u"主题", u"发表日期", u"发布平台", u"链接", u"链接2", u"分类", u"所属省份", u"所属市", u"作者", u"摘要"]

for idx, h in enumerate(head_row):
    worksheet.write(1, idx, h, center_format)
for idx, post in enumerate(posts, 2):
    for jdx, h in enumerate(head_row):
        if h == u"链接":
            worksheet.write_url(idx, jdx, post.get(h, ""), string=u"LINK", cell_format=blue_format)
        else:
            worksheet.write(idx, jdx, post.get(h, ""))
workbook.close()
filepath = self.filepath.split('cyberin_backend')[-1]
return filepath
```



### 设置列宽   

```python
worksheet.set_column(0, 12, 12)  # 0 到 12 列，列宽 12  

set_column(self, first_col, last_col, width=None, cell_format=None, options=None)
```


### 设置行高  

```python
set_row(0, 20)  # 第一行高度 20  

set_row(self, row, height=None, cell_format=None, options=None):    
```


#### 合并单元格  

```python
worksheet.merge_range(0, 0, 0, 12, u"美团安全政策类信息汇总{}-{}".format(self.args[u"date1"], self.args[u"date2"]), center_format)  # 左上 0,0 右下 0,12   

merge_range(self, first_row, first_col, last_row, last_col, data, cell_format=None):
```



### 高亮  

write_rich_string 某个词高亮，注意，使用 write_rich_string 的时候，如果写入的列表全部都是 string，没有 xlsxwriter format 对象，就会报错 AttributeError: 'str' object has no attribute '_get_xf_index'。    

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


单元格背景高亮    

`https://xlsxwriter.readthedocs.io/format.html#format-set-bg-color`       

```python
cell_format = workbook.add_format()

cell_format.set_bg_color('green')

worksheet.write('A1', 'Ray', cell_format)
```


# xlwt  

xlwt 是一个旧包，只能写入 xls 格式，xls 格式只能写入 65535 条数据。    

即使把后缀改成了 xlsx 也不行，治标不治本。      

xlwt 改 xlsxwriter 也很容易。   

原来：   
```python
f = xlwt.Workbook()
sheet1 = f.add_sheet(u'sheet1', cell_overwrite_ok=True)
f.save(filepath)
```

```python
workbook  = xlsxwriter.Workbook(filepath)
sheet1 = workbook.add_worksheet(u'sheet1')
workbook.close()
```




### 其他

```python 
import pandas as pd

df = pd.DataFrame({'Names':['Andreas', 'George', 'Steve',
                           'Sarah', 'Joanna', 'Hanna'],
                  'Age':[21, 22, 20, 19, 18, 23]})
```

```python 
In [1]: import pandas as pd

In [2]: li = [{'precision': 0.0, 'recall': 0.0, 'f1-score': 0.0, 'support': 2}, {'precision': 0.0, 'recall': 0.0, 'f1-score': 0.0, 'support': 2}]

In [3]: pd.DataFrame(li)
Out[3]: 
   precision  recall  f1-score  support
0        0.0     0.0       0.0        2
1        0.0     0.0       0.0        2
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

一个 django 命令小例子  

```python 
# -*- coding: utf-8 -*-
import time
import openpyxl
from django.core.management.base import BaseCommand
from interaction_clean import interaction_clean
from utils.async_call import AsyncCall
from utils.common import get_logger

logger = get_logger('fill_empty_account_id')


class Command(BaseCommand):

    def add_arguments(self, parser):
        pass

    def handle(self, *args, **options):
        start = time.time()
        upload_file = u'./data/如果科技员工相关舆情7月4日-8点.xlsx'
        wb = openpyxl.load_workbook(upload_file)
        sheet_content = wb[u'舆情']
        rows = sheet_content.max_row
        for i in range(2, rows + 1):
            url = sheet_content.cell(row=i, column=2).value
            if url:
                spider_result = interaction_clean.parse(url)
                print spider_result
        print '%.2f sec' % (time.time() - start)
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



## Excel 知识   

如果时间显示的是 float 格式，看是不是写到了两个 sheet 实例里，要改成同一个实例。     

日期是时间差 delta，起始时间是 1899.12.30，问 ChatGPT         


## 报错   

### Python2 read_excel 读表头取列因为 Unicode 编码问题报错 keyerror（8 次）   


Python2 取的时候，一律加 u      


读 Excel，遍历行的时候 KeyError 的一个解决办法   

```python
df.columns = [i.encode("utf-8").decode("utf-8") for i in df.columns]

实在不行就把中文改成英文。     
```



有一次是把列名前面的 u 去掉，就可以了，还是编码问题。    



最开始的写法     
```python 
import pandas as pd


df = pd.read_excel('./data/dlg_predict.xlsx')

df = df[df[u'话题'] != u'无意义']
print df
```

报错    

```python 
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-2: ordinal not in range(128)   
```

解决办法是改用循环     

```python  
df = pd.read_excel('./data/dlg_predict.xlsx')

df = df[[it != u'无意义' for it in df[u'话题']]]
print df
```




### Python3 read_csv 报错 UnicodeDecodeError: 'utf-8' codec can't decode byte 0xc8 in position 0: invalid continuation byte（2 次）

Python3 可以加 encoding 参数      

cyberin 的导出编码就是 gbk    

```python
df = pd.read_csv(newest_file, encoding='gbk')   
```

```python 
df = pd.read_csv(newest_file, encoding='GB18030')
``` 


### to_excel 报错 'ascii' codec can't decode byte 0xe5 in position 0: ordinal not in range(128)   

to_excel 没有 encoding 参数，要通过遍历，decode 数据。    

```python
# 比如 Date 里包含中文年月日，就可以先 decode 这个字段。  
df["Date"] = df["Date"].map(lambda x: x.decode('utf-8'))

# 处理表头
df.columns = df.columns.map(lambda x: x.decode('utf-8')) 
```

一个完整一些的写入的例子：   

```python
def write_to_excel(self, sheet_datas):
    filepath = self.get_filepath()
    with pd.ExcelWriter(filepath, engine='xlsxwriter', options={'strings_to_urls': False}) as writer:
        for sheet_name, sheet_data in sheet_datas.items():
            df = pd.DataFrame(sheet_data)[self.get_headers()]
            df.columns = df.columns.map(lambda x: x.decode('utf-8'))
            df["Date"] = df["Date"].map(lambda x: x.decode('utf-8'))
            df.to_excel(writer, sheet_name=sheet_name, index=False)
```


### to_csv 报错 'ascii' codec can't decode byte 0xe5 in position 0: ordinal not in range(128)

```python
# 加 encoding 参数
df.to_csv('beijingchezhan.csv', index=False, encoding='utf-8')
```


### 读 Excel，遍历行的时候 KeyError    

```python
df.columns = [i.encode("utf-8").decode("utf-8") for i in df.columns]
```



### XLRDError: Excel xlsx file； not supported 报错

原因是pip安装的是最新的 2.0.1 版本，只支持 .xls 文件。所以 pandas.read_excel(‘xxx.xlsx’) 会报错。   

可以安装旧版xlrd，在 cmd 中执行指令：    

```python
pip install xlrd==1.2.0
```


### UnicodeDecodeError: 'utf-16-le' codec can't decode bytes in position 1186-1187: unexpected end of data  

读 Excel 报错。   

xls 文件**另存为**（不是改后缀）成 xlsx 文件。    
如果不行，就新建一个空白的 xlsx 文件，然后从 xls 中复制内容，粘贴到 xlsx 文件中    
如果还不行，就安装 xlrd==1.2.0    

