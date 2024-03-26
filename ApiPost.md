
在 apipost 里复制原来的接口。      


1. 克隆原来的接口。
2. 重命名。
3. 复制网页上的请求。（GET 请求是这样，POST 请求把参数写到 body 里）（比如复制 `http://test.maixunbytes.com:8001/xposts/search/?date1=2024-03-25+00:00:00&date2=2024-03-26+23:59:59&include_time_1=&include_time_2=&src=-1&fid_arr=305987&page=1&hide_ontop=ALL&author=&is_noise=ALL&confident_low=&confident_high=&site=&urlseg=&titleseg=&dropdup=0&sdup=&sort=DATE&abstractseg=&titleseg_ang=&titleseg_none=&abstr_ang=&abstr_none=&display_model=web_page&max_count=200&is_origin=ALL&tit_abs=&content_type=ALL&author_type=ALL&post_type=0&media_type=ALL&include_post=project&asc=0&type=ALL&w_level=&post_obj_arr=&export=1&use_api=0`）    
4. 找到 header 标签
5. 复制页面上的 cookie。
6. 参数名里写 Cookie，参数值里写复制的值。
7. 请求测试。



请求：   

新建接口 -> 输入 url，比如：https://monitor.mxspider.top/msg_encryptor/link -> 选择请求方法，比如 POST -> POST 方法的参数写在 body 里，选择 Body 选项卡 -> 参数名 input-area -> 参数值 Hello world -> 发送     


文档：   

成功响应示例及参数描述 -> 从现有响应导入 -> 美化 -> 提取字段和描述 -> 右侧添加字段说明 -> Ctrl + s 保存 -> 分享文档    
