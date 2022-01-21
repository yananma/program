
PyCharm 配置 interpreter 的核心就是找到正确的 Python 所在的路径  


## 创建完整 Django 项目记录  

### 一、创建虚拟环境，安装 package    
`conda create --name zjgdk python=3.9` 

`pip install django==2.0`  


### 二、创建项目  
`django-admin startproject zjgdk`  
`django-admin startapp post`  


### 三、PyCharm 连接远程项目  
File -> New Project -> Pure Python -> 右侧点开 Project Interpreter -> Existing Interpreter -> 点击右侧三个点 -> SSH Interpreter -> Existing server configuration -> 点击下拉框  -> 点击选择 -> 点 Next 下一步 -> 右侧点击文件夹图标选择 /home/test/anaconda3/envs/环境名/bin/python -> 点击 finish -> 点击 Interpreter 下方 Remote project location 文件夹图标 -> 选择远程文件夹路径 -> 配置最上方 Location，修改文件夹名称 untitled 为项目名称 -> create； Tools -> Deployment 中取消勾选 Automatic upload    

右键项目文件夹，从远程 download 文件  

修改 interpreter 名称和远程连接名称：右下角点击左键 -> 选择 Interpreter Settings -> 点击右上边齿轮 -> 选择 show all -> 点击 Edit -> Name 就是 Interpreter 的 name；-> 下边 Deployment configuration -> 点击右边三个点 -> 右键点击左侧 -> rename  


### 四、运行项目配置  
点击右上角 Add Configuration -> 点击左上角加号 -> 选择 Django Server -> 左侧最上方 Name 改成 zjgdk -> Host 改为 0 -> Port 改为 6100 -> 中间 Environment variables 行点击最右边 -> 添加 DJANGO_SETTINGS_MODULE，值为 zjgdk.settings  




### 远程连接 Git 上已存在的项目   

配置远程环境：VCS -> Git -> Clone，clone 完成以后在 new window 打开  

在配置 python 解释器之前要先在 Tools -> Deployment 中取消 Automatic upload  

add python interpreter，选择 ssh interpreter，选择 existing server configuration，然后再选择正确的 python 解释器，配置正确的路径  


### PyCharm 切换分支  

先用在项目目录打开 git bash  

git pull 同步远程  

然后在命令行 git checkout 分支切换分支，PyCharm 会自动切换  


### 远程连接新建项目  

File -> New Project -> Pure Python -> 右侧点开 Project Interpreter -> 点击右侧三个点 -> Existing Interpreter -> SSH Interpreter -> Existing server configuration -> 点击下拉框  -> 点击选择 -> 点 Next 下一步 -> 右侧点击文件夹图标选择 /home/test/anaconda3/envs/环境名/bin/python -> 点击 finish -> 点击 Interpreter 下方文件夹图标 -> 选择远程文件夹路径，配置 Remote project location -> 配置最上方 Location，修改文件夹名称 untitled 为项目名称 -> create； Tools -> Deployment 中取消勾选 Automatic upload    





查找全部：Edit -> Find -> Find in Path  

替换全部：Edit -> Find -> Replace in Path   


### 快捷键

Ctrl + Alt + 方向键左键 跳回光标原来所在的位置；读源码用  

Shift + F10 运行  

Shift + ESC 隐藏命令结果行  

Ctrl + Shift + F10 运行当前页面(右上角选择的不是当前页面的时候)  

Alt + 1 显示隐藏当前目录  

Alt + 7 查看方法列表  

格式化代码样式  Ctrl + Alt + l  

切换 Alt + 方向键  


### 设置  

自动换行显示，View -> Active Editor -> Soft Wrap  

exclude 要下载的文件 Tools -> Deployment -> Options -> Exclude items by name 用正则匹配  

可以 debug，选择 dj 项目，有些是要在网页发送请求的时候才会触发  

可以在源码中打断点，可以结合源码书共同学习  

可以使用右上角的搜索功能，查找类和函数，读源码非常有帮助  

点进源码以后，点击左侧 Structure 可以看到所有的类；自己写 model 或 view 导包的时候，可以点进去看还有什么相似的方法       


PyCharm 在编写标签的时候，输入 if 按 Tab 键，就可以自动创建标签    

可以数值切分页面，这样就可以在写 CSS 的时候，可以看到 HTML 页面  

可以使用 PyCharm 连接 MySQL  

查找 Edit -> Find；用 Vim 的搜索功能    


项目是 pycharm 的最大的管理单元  

项目下面有包，包里面有各种模块  



#### 其他设置

pycharm 非常强大，可以创建 Django flask 项目、可以连接数据库、可以使用 git 进行版本控制、可以登录 GitHub、  

创建 Django 项目：File -> New Project -> Django -> Existing interpreter -> 右侧... -> Conda Environment -> 右侧... -> 上排 Show Hidden Files and Directories -> 选择 C:\ProgramData\Miniconda3\envs\django\python.exe -> 点击两次 OK -> 这个时候在 Existing interpreter 中就有了 django interpreter 了（但是会提示 installing django）  

打开 settings，可以使用快捷键 Ctrl + Alt + s，可以配置  

Alt + Enter 快速修复错误  

进入全屏：View -> Appearance -> Enter Full Screen    

PyCharm -> Tools -> HTTP Client -> Test RESTful Web Service 可以做简单测试    

连接数据库也非常简单，输入用户名密码和数据库名字即可  

[PyCharm 连接 MySQL 报错解决办法](https://blog.csdn.net/liuqiker/article/details/102455077)：`set global time_zone = '+8:00';`  


自动导入包设置：File -> Settings -> General -> Auto Import -> Python -> Show import popup  
导入包是 alt + enter 键组合，如果弹出下拉菜单选项，说明缺少依赖，选择即可导入（不知道为什么自己设置没有效果）  

取消更新提示：settings 里搜索 update，取消勾选即可  



## 旧笔记  

按住 ctrl ,再点击函数名称，就可以跳转到该函数的代码文件中  

可以查看源码，可以在源码中使用 Pysnooper  

改快捷键，debug：step into mycode 改成 F7，step out 改成 F6，step over 改成 F9   

### Docker 环境

启动实例 > 复制 IP > 

docker start -ai jpt  


service ssh start  
service ssh status  


打开 File > settings > project 名字 > Python interpreter > 右边点击齿轮 add > 选择 SSH Interpreter > 输入 IP 端口是 8022 > Username 是 root > Password ：1234567890 >  


上传到阿里云：大文件用 Xftp 传，先用 pycharm 传，知道目标文件夹，再删除已经上传的，进入 tmp 文件夹，找到 pycharm_project_xxx，进入以后 rm -rf 文件夹名，一个一个删除，再用 Xftp 传。  

pycharm 上传的方法：右键点击文件夹 > Deployment > Upload to root@IP  

配置参数：在 run 按钮的左边点开，选择 Edit Configurations > 如果目录不对要重新选择 Script path 点右侧文件夹图标 > 填 Parameters  



Tools > Deployment > Configuration > Mapping 的 Local path 转到当前文件夹下，而且必须指定 Deployment path，才能显示出 Deployment 选项，才能传数据到服务器  

File > Settings > 找 Python Interpreter 下的 Path mappings 点击右侧文件夹图标，添加。这个是文件夹同步的目录  


右侧显示 Remote Host：Tools > Deployment > Browse Remote Host   




### Miniconda 环境

#### Windows 

要创建虚拟环境：conda create -n maskrcnn python==3.6.3  

右下角 Interpreter Settings > 齿轮 > show all > + > 选择 Conda Environment > Existing environment > 



#### Linux 

在服务器端配置完成环境以后打开 pycharm  

可以直接在右下角添加 Interpreter  

打开 File > settings > project 名字 > Python interpreter > 右边点击齿轮 add > 选择 SSH Interpreter > 输入 IP 端口就是 22 > Username 是 root > Password 暗文：自己名字自己邮箱号自己的手机号 > 选择 Interpreter：在服务器端 conda env list 看地址，然后点击右边文件夹图标选，要一直选到 bin 下面的 python.exe 为止 

退出 conda 环境：conda deactivate  



上传到阿里云：大文件用 Xftp 传，先用 pycharm 传，知道目标文件夹，再删除已经上传的，再用 Xftp 传。  

pycharm 上传的方法：右键点击文件夹 > Deployment > Upload to root@IP  

配置参数：在 run 按钮的左边点开，选择 Edit Configurations > 如果目录不对要重新选择 Script path 点右侧文件夹图标 > 填 Parameters  



### debug：不会 debug 根本就不可能学会编程  

debug 是方法，可以解决几百万个问题  

如果学 Django 的时候会 debug 能解决多少问题？能省多少时间？会容易得多  

1、打开了读源码的大门，前景无限  
2、以后可以学习公司业务  
3、编程中 debug 时间最多  
4、不用再去依赖别人  
5、可以提高自己独立解决问题，理解问题的能力  
6、没有 debug 功能会极为麻烦，自己试过，拆分组合，极为复杂，效果甚微  






