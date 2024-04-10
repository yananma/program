
在 apipost 里复制原来的接口。      


1. 克隆原来的接口。
2. 重命名。
3. 复制网页上的请求。（GET 请求是这样，POST 请求把参数写到 body 里）
4. 比如复制 `http://test.maixunbytes.com:8001/xposts/search/?date1=2024-03-25+00:00:00&date2=2024-03-26+23:59:59&include_time_1=`    
5. 找到 header 标签
6. 复制页面上的 cookie。
7. 参数名里写 Cookie，参数值里写复制的值。
8. 请求测试。



请求：   

新建接口 -> 输入 url，比如：https://monitor.mxspider.top/msg_encryptor/link -> 选择请求方法，比如 POST -> POST 方法的参数写在 body 里，选择 Body 选项卡 -> 参数名 input-area -> 参数值 Hello world -> 发送     


POST 请求可以传 json 格式的参数。     



文档：   

成功响应示例及参数描述 -> 从现有响应导入 -> 美化 -> 提取字段和描述 -> 右侧添加字段说明 -> Ctrl + s 保存 -> 分享文档    
