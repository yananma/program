
# 筛选完一个，自己手写一遍底下的查询语句。   

在列表是 id 在列表 80, 74, 59, 66, 78，没有外面的一层 []    

如果是查 url 列表，url 元素要弄成字符串格式。    


## 配置   

配置数据库：连接 -> MySQL -> 配置常规和 SSH   

看 settings 找数据库配置。   

SSH 配置：  

    112.*.*.39
    17***  
    deploy  
    复制密码   


在设计表的时候，在复制别的表的时候，复制的时候通过最左边很窄的那一列空白列复制，最左边的那一列显示黑点，这样复制的才有结构。如果直接复制右侧的 cell，复制出来的就是一个字符串，没有表结构。     

在目标表里粘贴的时候，只选中第一个 cell。   

复制表到另一台服务器的数据库：找到数据表右键 -> 转储 SQL 文件 -> 结构和数据 -> 保存（不用再点 Navicat 弹框的按钮）-> 在目标数据库上右键 -> 运行 SQL 文件   


# 知识点   


### COUNT   

```sql 
SELECT COUNT(*) FROM `community`.`author` WHERE `site_id` = '2'
```

### ORDER BY  

```sql 
SELECT * FROM `xpost` WHERE domain = '东方财富网-股吧' AND `title` LIKE '%>%' ORDER BY `updatetime` DESC   
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

### RIGHT  

```
SELECT * FROM xpost WHERE domain LIKE '%汽车之家%' AND RIGHT(title, 2(这个数字就是等号后面的字符串的 len))='论坛' ORDER BY include_t DESC
```


### DELETE   

```sql  
DELETE FROM `community`.`author_clean` WHERE `site_id` = '2'
```   


# 语句   

### 在列表   

```sql 
SELECT * FROM `notice`.`keyword_warning` WHERE `id` IN (80, 74, 59, 66, 78) LIMIT 0,1000   
```



# 报错   

### 1241 - Operand should contain 1 column(s)   

查询语句：   

```sql 
SELECT DISTINCT siteid FROM `notice`.`xpost` WHERE `facetid` = (59, 66, 74, 75, 76, 77, 78) AND `include_t` >= '2022-05-31 00:00:00' AND `include_t` <= '2022-06-03 23:59:59' AND `domain` = '抖音'  
```

要把 facetid 的 = 改成 in。   
