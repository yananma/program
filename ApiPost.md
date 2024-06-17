
在 apipost 里复制原来的接口。      


1. 克隆原来的接口。
2. 重命名。
3. 复制网页上的请求。（GET 请求是这样，POST 请求把参数写到 body 里）
4. 比如复制 `http://test.maixunbytes.com:8001/xposts/search/?date1=2024-03-25+00:00:00&date2=2024-03-26+23:59:59&include_time_1=`    
5. 找到 header 标签
6. 复制页面上的 cookie。
7. 参数名里写 Cookie，参数值里写复制的值。
8. 请求测试。




### GET 请求  




### POST 请求   

复制一个 POST 请求接口   
在 Header 里填写 Cookie，参数名填写字符串 `Cookie`，参数值写请求的 header 里的值     
在 Body 里填写请求参数。先在浏览器的 Payload 里复制参数（复制很多行，每一种是键值对那一种形式，用鼠标左键拉动复制），在 ApiPost 里选择 multipart/form-data 形式，从左边选择导入参数，然后粘贴，导入。   



### 文档   

成功响应示例及参数描述 -> 从现有响应导入 -> 美化 -> 提取字段和描述 -> 右侧添加字段说明 -> Ctrl + s 保存 -> 分享文档    
