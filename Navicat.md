
以后全部手写 SQL 语句，写 10000 条就差不多了。每 100 条，备份一次。    

在列表是 id 在列表 80, 74, 59, 66, 78，没有外面的一层 []    

如果是查 url 列表，url 元素要弄成字符串格式。    


## 配置   

**在左侧右键 51 测试服务器，复制连接，只修改主机就可以了，改成内网编号。** 如果报错说不能从某台服务器跳转，那就手动填写，不通过复制创建。          

配置数据库：连接 -> MySQL -> 配置常规和 SSH   

看 settings 找数据库配置。   

SSH 配置：  

    112.*.*.39
    17***  
    deploy  
    复制密码   


在设计表的时候，在复制别的表的时候，复制的时候通过最左边很窄的那一列空白列复制，最左边的那一列显示黑点，这样复制的才有结构。如果直接复制右侧的 cell，复制出来的就是一个字符串，没有表结构。     

在目标表里粘贴的时候，只选中第一个 cell。   

复制表到另一台服务器的数据库：直接在一个数据库的对象 tab 下 Ctrl + c 复制表，在另一个库里的对象 tab 下 Ctrl + v 粘贴就行。（或找到数据表右键 -> 转储 SQL 文件 -> 结构和数据 -> 保存（不用再点 Navicat 弹框的按钮）-> 在目标数据库上右键 -> 运行 SQL 文件）   


## 手动创建表  

models 里，没有写 id，要创建表的时候要加上 id，而且要自动递增    

外键，加字段的时候要加上 字段_id 字段，数据库外键不用弄。    

float 字段，类型是 double，长度 0，精度 0.     

IntegerField 只有 0 和 1 的，用 tinyint，长度 2. 如果有多个，就用 int，长度 11.     


对照表   

```python  
    data_types = {
        "AutoField": "integer AUTO_INCREMENT",
        "BigAutoField": "bigint AUTO_INCREMENT",
        "BinaryField": "longblob",
        "BooleanField": "bool",
        "CharField": "varchar(%(max_length)s)",
        "DateField": "date",
        "DateTimeField": "datetime(6)",
        "DecimalField": "numeric(%(max_digits)s, %(decimal_places)s)",
        "DurationField": "bigint",
        "FileField": "varchar(%(max_length)s)",
        "FilePathField": "varchar(%(max_length)s)",
        "FloatField": "double precision",
        "IntegerField": "integer",
        "BigIntegerField": "bigint",
        "IPAddressField": "char(15)",
        "GenericIPAddressField": "char(39)",
        "JSONField": "json",
        "OneToOneField": "integer",
        "PositiveBigIntegerField": "bigint UNSIGNED",
        "PositiveIntegerField": "integer UNSIGNED",
        "PositiveSmallIntegerField": "smallint UNSIGNED",
        "SlugField": "varchar(%(max_length)s)",
        "SmallAutoField": "smallint AUTO_INCREMENT",
        "SmallIntegerField": "smallint",
        "TextField": "longtext",
        "TimeField": "time(6)",
        "UUIDField": "char(32)",
    }
```




# 知识点   

### REGEXP 

```sql 
SELECT title, domain, url FROM xpost WHERE domain LIKE '%搜狐%' AND title REGEXP '^原创[ \t]*原'

# 匹配 [百度贴吧] 要用两个斜线   
SELECT facetid, url, title, posttime, `status`, domain FROM xpost WHERE domain = '百度贴吧' AND title REGEXP '\\[(.*?)\\]'
``` 

### 索引  

```sql 
SELECT facetid, url, title, abstract, posttime, include_t, `status`, entry_name FROM xpost FORCE KEY  (`idx_fid`) WHERE title LIKE "%也长锈，漆面起泡泡%" AND facetid = 74
``` 


### COUNT   

```sql 
SELECT COUNT(*) FROM `community`.`author` WHERE `site_id` = '2'
```

### ORDER BY  

```sql 
SELECT * FROM `xpost` WHERE domain = '东方财富网-股吧' AND `title` LIKE '%>%' ORDER BY `updatetime` DESC   
``` 

### GROUP BY 

```sql 
SELECT domain, COUNT(domain) FROM xpost WHERE domain LIKE '%搜狐%' AND title LIKE '%原创%' GROUP BY domain
``` 


### DISTINCT   

查询 domain 是抖音的 siteid 都有哪些。   

```sql   
SELECT DISTINCT siteid FROM `notice`.`xpost` WHERE `facetid` IN (59, 66, 74, 75, 76, 77, 78) AND `include_t` >= '2022-05-31 00:00:00' AND `include_t` <= '2022-06-03 23:59:59' AND `domain` = '抖音'
```

或者用 group by   

```sql   
SELECT siteid FROM `notice`.`xpost` WHERE `facetid` IN (59, 66, 74, 75, 76, 77, 78) AND `include_t` >= '2022-05-31 00:00:00' AND `include_t` <= '2022-06-03 23:59:59' AND `domain` = '抖音' GROUP BY siteid   
```


```sql 
SELECT Count(DISTINCT author) FROM `xpost` WHERE `siteid` = '2'
```


### LIMIT    

``` 
当数据库查询记录有几万、几十万时使用limit查询效率非常快。   

select * from tableName limit i,n

i：为查询结果的索引值(默认从0开始)，当i=0时可省略i    
n：为查询结果返回的数量   

select * from Customer limit 10;   --检索前10行数据，显示1-10条数据
select * from Customer limit 5,10; --检索从第6行开始向前加10条数据，共显示id为6,7....15
```   


### LEFT  

```sql 
SELECT title, domain FROM xpost WHERE domain LIKE '%搜狐%' AND title LIKE '%原创%' AND LEFT(title,2) != '原创' 
``` 


### RIGHT  

```sql
SELECT * FROM xpost WHERE domain LIKE '%汽车之家%' AND RIGHT(title, 2(这个数字就是等号后面的字符串的 len))='论坛' ORDER BY include_t DESC
```


### DELETE   

```sql  
DELETE FROM `community`.`author_clean` WHERE `site_id` = '2'
```   

当要删除很多个 id 的时候，不要用 in 删，这么删会非常非常慢。用多条语句删。    

错误做法   

```sql   
DELETE FROM xpost WHERE entryid in (4210981, 4214913, 4219581, 4222792, 4273673, 4273803, 4273596, 4273661, 4273737, 4273626, 4273795, 4273819, 4273777, 4273617, 4273651, 4273589, 4273587, 4273584, ...) AND type_reason = '' AND is_comment = 1
```

正确做法    

```sql   
delete FROM xpost WHERE entryid=4273777 AND type_reason = '' AND is_comment = 1;
delete FROM xpost WHERE entryid=4273617 AND type_reason = '' AND is_comment = 1;
delete FROM xpost WHERE entryid=4273651 AND type_reason = '' AND is_comment = 1;
delete FROM xpost WHERE entryid=4273589 AND type_reason = '' AND is_comment = 1;
delete FROM xpost WHERE entryid=4273587 AND type_reason = '' AND is_comment = 1;
...
```


# 语句   

### 是 None   

```sql 
IS NULL
```


### 在列表   

```sql 
SELECT * FROM `notice`.`keyword_warning` WHERE `id` IN (80, 74, 59, 66, 78) LIMIT 0,1000   
```

### 统计类别和类别的数量   

```sql 
SELECT domain, COUNT(domain) FROM xpost WHERE domain LIKE '%搜狐%' AND title LIKE '%原创%' GROUP BY domain
```


### 按时间聚类   

分钟是 %i，不是 %m    
```sql  
# 按小时  
select DATE_FORMAT(include_t,'%Y-%m-%d %H') hours, count(postid) count from xpost WHERE include_t >= '2022-10-01 00:00:00' group by hours ORDER BY include_t;

# 按天  
select DATE_FORMAT(include_t,'%Y-%m-%d') days, count(postid) count from xpost WHERE include_t >= '2022-10-01 00:00:00' group by days ORDER BY include_t;
```


# 报错   

### 1241 - Operand should contain 1 column(s)   

查询语句：   

```sql 
SELECT DISTINCT siteid FROM `notice`.`xpost` WHERE `facetid` = (59, 66, 74, 75, 76, 77, 78) AND `include_t` >= '2022-05-31 00:00:00' AND `include_t` <= '2022-06-03 23:59:59' AND `domain` = '抖音'  
```

要把 facetid 的 = 改成 in。   
