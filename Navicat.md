

SQL 是更本质的查询，遇到难以排查的问题就看 SQL 语句。       


以后全部手写 SQL 语句，写 10000 条就差不多了。    


从光标选到列尾：Ctrl + Shift + End。       


### 知识点   

* 数量多的时候，navicat 加载结果也要花很多时间，所以这种情况下使用 limit 就会非常快。   



### 杀多个进程  

```sql
kill tidb 5465694291910743283; 
kill tidb 5465694291910749343; 
```



## 帮助命令   

`help` 或者 `help content`    



### 查询数据库都有什么字段   

```python
 show columns from xentry
```

只展示字段    

```sql   
SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA = 'hot_list' AND TABLE_NAME = 'bilibili';
```


### 添加时间列  

```sql
ALTER TABLE user_obj
ADD COLUMN create_time DATETIME DEFAULT CURRENT_TIMESTAMP;


ALTER TABLE user_obj
ADD COLUMN update_time DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP;
```

### 删除列  

```sql
ALTER TABLE table_name DROP COLUMN column_name;
```


# 项目   

## Cyberin     


按 sourcetype 统计数量

通用的 fid：304265, 304266, 305987, 306022, 306023, 306024, 306025, 306026, 306027, 306028, 306032, 306036, 306043, 306044, 306046, 306054, 306055, 306056, 306057, 306058, 306059, 306060, 306061, 306062, 306063, 306064, 306065, 306066, 306067, 306070, 306071, 306072, 306111, 306114, 306115, 306116, 306148, 306150, 306239, 306240, 306241, 306242, 306243, 306244, 306245

```python  
SELECT sourcetype, COUNT(1) AS cnt FROM xpost WHERE entryid IN (5113547,5113621,5114626) AND hidden IN (-2, -1, 0, 2, 3, 4) AND is_comment = 0 GROUP BY sourcetype ORDER BY cnt DESC    
```


## 央视热榜推送  

看数量够不够 50 条：    

```sql
SELECT version, COUNT(*) AS count, create_time FROM `weibo` WHERE version IN (359105, 359106, 359107) GROUP BY version ORDER BY create_time DESC   
```



以一条 SQL 的查询结果，作为另一条 SQL 的查询条件   

```sql
SELECT
	version,
	COUNT(*) AS count,
	create_time 
FROM
	`weibo` 
WHERE
	version IN ( SELECT version FROM `hot_list`.`weibo` WHERE `topic` = '普京访华' ORDER BY `version` ) 
GROUP BY
	version 
ORDER BY
	create_time DESC
```



## 央视热搜，看是否是榜首   

```sql
SELECT topic, MIN(`rank`) FROM weibo WHERE topic IN ("黑龙江一煤矿重大事故调查结果", "川渝火锅在链博会有一个专门展览", "46周岁以上居民身份证长期有效", "身份证18位数字分别代表什么", "停火之前以军对黎发动最猛烈袭击", "黎以停火协议今日10时生效", "黎以停火协议已被批准", "女子抵押房子也要给骗子送钱", "大连失联船舶已找到", "下一个中国还是中国", "军人一遍遍演练动作迎接烈士遗骸", "刘连舸一审被判死缓", "川渝火锅进入全球80多个国家") GROUP BY topic 
```



## 危机预警  

索引查询    

```python  
SELECT * FROM `xpost` FORCE KEY  (`idx_eid`)  WHERE (`xpost`.`hidden` = 0 AND `xpost`.`entryid` IN (123985) AND `xpost`.`include_t` >= '2022-10-31 00:00:00' AND `xpost`.`status` IN (2) AND `xpost`.`noise_rank` = 0) ORDER BY posttime ASC   
```

比亚迪按天聚合统计数量   

```python 
select DATE_FORMAT(include_t,'%Y-%m-%d') AS days, count(postid) AS count from xpost WHERE (`xpost`.`hidden` = 0 AND `xpost`.`include_t` >= '2022-10-01 00:00:00' AND `xpost`.`status` IN (2) AND `xpost`.`noise_rank` = 0 AND `xpost`.`facetid` = 59) group by days ORDER BY include_t;
```




# 多试   

### SQL 知识点   

1. limit 在 order by 后面     


### 实践知识点    

1. 从 Django 的 SQL 里复制出来的 SQL 在 Navicat 里试，要加引号。已经犯了好多次这个错误了。     
2. 在列表是 id 在列表 80, 74, 59, 66, 78，没有外面的一层 []    
3. 如果是查 url 列表，url 元素要是字符串格式。'http:1','http:2'    


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



### 杀查询进程  

```python  
show PROCESSLIST

show full PROCESSLIST    

在 info 列里找 SQL，找到以后复制 id      

MySQL：kill 2177771
tidb：kill tidb 2177771 
```


### 查询占用时间长

可以直接查 INFORMATION_SCHEMA 的 SLOW_QUERY 表，可以导出来看。按 query time 排序。    

SELECT * FROM INFORMATION_SCHEMA.CLUSTER_PROCESSLIST ORDER BY TIME DESC 查看花费时间长的 SQL 进程。


### 在改数据库字段，或者 drop table 很慢的时候，可能是有进程在 waiting   

用 show processlist 看有没有进程在 waiting，如果有进程就杀掉     

drop table 是很快的，上千万数据，马上就能 drop 掉。如果 drop 了很长时间，很可能就是有进程在 waiting     


### tidb查某个服务器请求的 SQL  

```python
select * from INFORMATION_SCHEMA.processlist where host like '192.168.241.70%';
select * from INFORMATION_SCHEMA.cluster_processlist where host like '192.168.241.70%';
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

危机预警统计某一个月的数据量。    

```sql  
select COUNT(1) FROM xpost WHERE hidden = 0 AND facetid IN (59, 66, 74, 75, 76, 77, 78) AND posttime >= '2022-09-01 00:00:00' AND posttime <= '2022-09-30 23:59:59' AND status IN (0, 1) AND noise_rank = 0
```  


### ORDER BY  

```sql 
SELECT * FROM `xpost` WHERE domain = '东方财富网-股吧' AND `title` LIKE '%>%' ORDER BY `updatetime` DESC   
``` 

### GROUP BY 

```sql 
SELECT domain, COUNT(domain) FROM xpost WHERE domain LIKE '%搜狐%' AND title LIKE '%原创%' GROUP BY domain
``` 

Cyberin 统计正负面    

```sql  
SELECT type, COUNT(type) FROM `xpost` WHERE facetid = '306032' AND posttime >= '2022-10-01 00:00:00' AND posttime <= '2022-10-15 23:59:59' GROUP BY type 
```

如果 as 了中文，group by 和 order by 的时候要加转码    

```sql
SELECT facetid, is_comment, DATE_FORMAT(posttime,'%Y-%m') AS '发文月份', COUNT(*) AS '数量' FROM xpost WHERE entryid IN (5992259, 5999921) AND is_comment = 1 AND hidden IN (-2, -1, 0) GROUP BY `发文月份` ORDER BY `发文月份`  
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


### CASE   

```sql
SELECT DISTINCT(url) AS "链接", author_location AS "地域", CASE WHEN gender = -1 THEN ""
	WHEN gender = 0 THEN "女"
	WHEN gender = 1 THEN "男"
END
 AS "性别", posttime AS "发文时间" FROM `xpost` WHERE url IN ("http://www.dongchedi.com/article/17823191939") ORDER BY posttime 
```

### IF  

```sql
SELECT DISTINCT(url) AS "链接", author_location AS "地域", IF(gender = -1,"",IF(gender = 1,"男","女"))
 AS "性别", posttime AS "发文时间" FROM `xpost` WHERE url IN ("http://www.dongchedi.com/article/17823191939") ORDER BY url
```


```sql
SELECT
	count(*),
(	CASE
	WHEN (type_rank >= 20) THEN
		"负面"
	WHEN (pos_type_rank >= 1) THEN
		"正面"
	ELSE
		"中立"
		END
) AS "调性"
FROM
	`xpost` 
WHERE
	entryid IN (5265914, 5269200) 
GROUP BY
	`调性`
```


```sql
SELECT
	count(*),
IF
	(
		type_rank >= 20,
		"负面",
	IF
	( pos_type_rank >= 1, "正面", "中立" )) AS "调性" 
FROM
	`xpost` 
WHERE
	entryid IN ( 5509358 ) 
GROUP BY
	`调性`
```


### LEFT  

```sql 
SELECT title, domain FROM xpost WHERE domain LIKE '%搜狐%' AND title LIKE '%原创%' AND LEFT(title,2) != '原创' 
``` 


### RIGHT  

```sql
SELECT * FROM xpost WHERE domain LIKE '%汽车之家%' AND RIGHT(title, 2(这个数字就是等号后面的字符串的 len))='论坛' ORDER BY include_t DESC
```




### INSERT INTO 

```python
INSERT INTO topic_aspect_rank  （表名）
(fid, postid, topic, name, `date`)  （字段）
VALUES (307614, 101465535729, 'xwh123', 'Celestiq', '2023-10-23'),   （数据）
       (307614, 101461509500, '品牌', 'Celestiq', '2023-10-23') 
ON DUPLICATE KEY UPDATE topic = VALUES(topic)  （要更新的字段）
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



### UPDATE  

```sql
UPDATE Product SET sale_price=sale_price ＊ 10 WHERE product_type=’厨房用具’;      
```




### MySQL 的 split   

select SUBSTRING_INDEX(SUBSTRING_INDEX('40,50',',',1),',',-1);结果为 40      

select SUBSTRING_INDEX(SUBSTRING_INDEX('40,50',',',2),',',-1);结果为 50      

函数解析：SUBSTRING_INDEX（str, delim, count）      

参数解说     解释      
str 　　　　需要拆分的字符串      
delim 　　  分隔符，通过某字符进行拆分      
count 　　  当 count 为正数，取第 n 个分隔符之前的所有字符； 当 count 为负数，取倒数第 n 个分隔符之后的所有字符。      

```sql  
SELECT SUBSTRING_INDEX(HOST,":",1) AS server, COUNT(1) AS c FROM information_schema.PROCESSLIST GROUP BY server ORDER BY c DESC      
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

```sql 
DATE_FORMAT(include_t,'%Y-%m-%d %H:%i:%s')   
```


分钟是 %i，不是 %m    
```sql  
# 按小时  
select DATE_FORMAT(include_t,'%Y-%m-%d %H') hours, count(postid) count from xpost WHERE include_t >= '2022-10-01 00:00:00' group by hours ORDER BY include_t;

# 按天  
select DATE_FORMAT(include_t,'%Y-%m-%d') days, count(postid) count from xpost WHERE include_t >= '2022-10-01 00:00:00' group by days ORDER BY include_t;
```

按天聚类，再按字段中的类型聚类   

```sql
SELECT DATE_FORMAT(posttime,'%Y-%m-%d') AS days, 
SUM(CASE WHEN is_comment = 0 THEN 1 ELSE 0 END) AS post,
SUM(CASE WHEN is_comment = 1 THEN 1 ELSE 0 END) AS `comment` 
FROM xpost WHERE entryid IN (5772605, 5775742, 5781123) GROUP BY days ORDER BY days
```

通用统计主贴 + 互动数的评论数  

```sql
SELECT DATE_FORMAT(posttime,'%Y-%m-%d') AS days, 
COUNT(*) + SUM(reply) AS cnt 
FROM xpost WHERE entryid IN (6330204, 6330216) AND is_comment = 0 GROUP BY days ORDER BY days
```



# sqlite   

和 MySQL 一样，当做 MySQL 用。可以在 PyCharm 里导入使用。自己的 navicat 里不能用，但是别的版本可以用，因为自己的只支持 MySQL。db_viewer 也可以用。    

```python
import sqlite3

DB_NAME = 'updata_to_xpost_task_info.db'

con = sqlite3.connect(DB_NAME, isolation_level=None)

cursor = con.cursor()

cursor.execute("PRAGMA table_info(task_info);")

columns = cursor.fetchall()
for column in columns:
    print(column)

cursor.execute("select * from task_info where name = '微信_20240618194224913993.xlsx'")

rows = cursor.fetchall()
for row in rows:
    print(row)

cursor.close()
con.close() 
```






# 报错   

### 1241 - Operand should contain 1 column(s)   

查询语句：   

```sql 
SELECT DISTINCT siteid FROM `notice`.`xpost` WHERE `facetid` = (59, 66, 74, 75, 76, 77, 78) AND `include_t` >= '2022-05-31 00:00:00' AND `include_t` <= '2022-06-03 23:59:59' AND `domain` = '抖音'  
```

要把 facetid 的 = 改成 in。   


### 1064 - You have an error in your SQL syntax;   

在使用筛选功能的在列表查询的时候，列表中的元素要加引号，否则报这个错。    

错误用法： `url 在列表 https://www.douyin.com/video/7155526446100794655,https://www.douyin.com/video/7155620716644273445`    
正确用法： `url 在列表 'https://www.douyin.com/video/7155526446100794655','https://www.douyin.com/video/7155620716644273445'`     

```python
print("','".join(s.splitlines()))    
```

### 加字段一直加不上，用 SHOW PROCESSLIST 看进程，状态显示 Waiting for table metadata lock   

可能是死锁了，去百度上搜，说可以用 `SELECT * FROM sys.schema_table_lock_waits` 命令看是不是死锁了。不过自己没试过。      

因为原来的表有很多任务在读，所以加字段进程没有办法获取独占锁，所以就会一直等待。    

遇到这种情况，或者没有遇到这种情况也可以，如果表不大的话，可以先复制一份，然后在复制的表上加字段，然后再把原来的表删除，然后再重命名复制表为原来表的名字。     




# Navicat 报错


### 报错 2003 - Can't connect to MySQL server on '127.0.0.1' (10061 "Unknown error")    

关闭连接 -> 编辑连接重命名 -> 复制连接命名为原来的名字 -> 删除原来的连接     



### 报错 access denied @b70 

端口变了。    






