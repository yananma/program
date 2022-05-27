
# 筛选完一个，自己手写一遍底下的查询语句。   

在列表是 id 在列表 80, 74, 59, 66, 78，没有外面的一层 []    


配置数据库：连接 -> MySQL -> 配置常规和 SSH   

看 settings 找数据库配置。   


在设计表的时候，在复制别的表的时候，复制的时候通过最左边很窄的那一列空白列复制，这样复制复制的才有结构。如果直接复制右侧的 cell，复制出来的就是一个字符串，没有表结构。     

在目标表里粘贴的时候，只选中第一个 cell。   




# 知识点   


### COUNT   

```sql 
SELECT COUNT(*) FROM `community`.`author` WHERE `site_id` = '2'
```


### DISTINCT   

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



### DELETE   

```sql  
DELETE FROM `community`.`author_clean` WHERE `site_id` = '2'
```   


# 语句   

### 在列表   

```sql 
SELECT * FROM `notice`.`keyword_warning` WHERE `id` IN (80, 74, 59, 66, 78) LIMIT 0,1000   
```
