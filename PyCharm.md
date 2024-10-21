

如果 debug 因为编码问题报错 keyerror，就可以用 decode(u"utf-8") 转换编码。    
通过切换右下角的 Python interpreter 来实现重新加载新的包，而不用删文件夹再重新打开。   
可以在 debug 过程中改变量的值，要经常用这种方法，而不是总是重新进入，或者复制各种很难的数据，应该自己创造假数据。   
把正在写的功能的 tab 先 pin 起来，不用再找了    
Ctrl + d 复制    
Ctrl + Shift + ↑ 移动语句    
Shift + Tab 反向移动    
debug 的 window 在左侧有一个 pause program，可以中断执行     
可以用 Change Signature 来改变参数顺序。     
pycharm 右键有 generate test      
简单的 oid、fid 处理，就在 pycharm 里用鼠标操作，不用打开 EmEditor   
PyCharm 的书签用冒泡法排序。     
重命名不用选中     
git log 可以按人、按文件搜索。        
git log 可以搜索 commit message。      
git show history 可以看到这个文件的 history    
git show history for selection 可以看到选中的代码的提交记录。    
Python console 里可以看到执行过的命令的历史记录。Ctrl + a 可以选择全部。       
在搜索旁边有一个 Find Tool Window      


通过 debug 进 Django 源码学会了 Django 的 call_command 的用法。      



PyCharm 配置的核心就是两个：   
1. 一个是 server，在 Tools -> Deployment -> Configuration 中，这个是上传下载文件用的。    
2. 一个是 interpreter，在右下角，配置 interpreter 的核心就是找到正确的 Python 所在的路径，里面有 server 是 python 的 server。就用上面那一步的 server 就可以了。   


## 创建完整 Django 项目记录  

`conda create --name zjgdk python=3.9` 

`pip install django==2.0`  

`django-admin startproject zjgdk`  
`cd zjgdk`  
`django-admin startapp post`  


## 新建项目配置，自己从零创建的   

File -> New Project -> Pure Python -> 右侧点开 Project Interpreter -> Existing Interpreter -> 点击右侧三个点 -> SSH Interpreter -> （如果不存在就新建，输入 IP、端口、用户名、密码、测试连接，选择虚拟环境的 Python 解释器）如果存在 Existing server configuration -> 点击下拉框  -> 点击选择 (如果报错，看是不是开了 VPN)-> 点 Next 下一步 -> 右侧点击文件夹图标选择 /home/test/anaconda3/envs/环境名/bin/python -> 点击 finish -> 点击 Interpreter 下方 Remote project location 文件夹图标 -> 选择远程文件夹路径 -> 配置最上方 Location，修改文件夹名称 untitled 为项目名称 -> create； Tools -> Deployment 中取消勾选 Automatic upload    

如果是新建的 SSH Interpreter，如果配置完成以后，deployment 是灰色的，就要配置 Tools -> Deployment -> Configuration，左侧列表中选择名字，右侧配置 Connection 的 Type 和 SSH configuration下边不用配置（一般默认已经配置了） ；配置 Mappings 的 Deployment path 为远程路径，选择正确的文件夹  

Tools -> Deployment -> Options 添加 exclude 文件类型：`.svn;.cvs;.idea;.DS_Store;.git;.hg;*.hprof;*.pyc`，单个形式为：`;*.jpg`，复制粘贴多次，然后替换 jpg  

右键项目文件夹，从远程 download 文件。如果文件比较大，最好自己弄成压缩包，用 sz 传输，然后自己在本地文件夹下面解压，不要通过 PyCharm 下载，很慢。       



## 从 gitlab 上 clone 项目     

先 clone 项目：PyCharm -> Git -> Clone      

如果 clone 下来只有一个空文件夹，没有项目内容，就连远程，然后 download。     







## 已经有的项目配置，比如刚 git clone 下来的项目   

### Interpreter   

添加 Interpreter：右下角 -> Add Interpreter -> SSH Interpreter -> 如果存在 Existing server configuration -> Next -> 选择 Python -> 配置路径 -> 取消 auto upload。

修改 interpreter 名称和远程 server 连接名称：右下角点击左键 -> 选择 Interpreter Settings -> 点击右上边齿轮 -> 选择 show all -> 点击 Edit -> Name 就是 Interpreter 的 name；-> 下边 Deployment configuration -> 点击右边三个点 -> 右键点击左侧 -> rename  

在配置 Interpreter 的时候就配好了 server，就不用再单独配置 server。   

如果没有就要配置 server。   




### 添加 server   

Tools -> Deployment -> Configuration，左上角 + 号，选择 SFTP，配置 Connection 和 Mappings。   

Connection 配置 SSH configuration，Mappings 配置本地和远程路径。    


### 删除上传时提示的不需要的 server   

Tools -> Deployment -> Configuration，左上角 - 号。   


### Django 项目配置  

配置 mapping，不配置会报错说找不到 manage.py：右下角点击左键 -> 选择 Interpreter Settings -> 配置 Path mappings -> 配置 local 和 remote 路径    


点击右上角 Add Configuration -> 点击左上角加号 -> 选择 Django Server -> 左侧最上方 Name 改成 zjgdk -> Host 改为 0 -> Port 改为 6100 -> 中间 Environment variables 行点击最右边 -> 添加 DJANGO_SETTINGS_MODULE，值为 zjgdk.settings  

如果不能运行的话，要配置 Django support，File -> Settings -> Language & Frameworks -> Django 勾选 Enable Django Support，配置 Django project root 为本地路径，配置 Manage script 为本地 manage.py   


### 找不到本地 python 解释器  

尽量不在本地开发，没法上传，会打乱上传代码的习惯。   

左键右下角 -> Add Interpreter -> 左边就是默认的 Virtualenv Environment -> 右边选择 Existing environment -> 右边三个点 -> 上面一排 icons，看最后一个 Show Hidden Files and Directories -> 然后就可以看到 AppData 这样原来被隐藏的文件夹了，然后就是找 Python 解释器就可以了。         


### 本地安装包版本太旧，更新版本   

版本不对，源码就不对，读源码就会有问题，debug 就会有问题，所以要更新。    

先通过一个函数进入源码 -> 右键上面文件名 open in -> 选 Explorer -> 删除本地的包 -> 重启 pycharm（不是关了所有的项目，关当前项目就可以） 就会自动安装远程最新的包    


### console 配置 settings   

File -> Settings -> Build Execution Deployment -> Console -> Django Console -> Environment Variables 中添加 DJANGO_SETTINGS_MODULE=aima_monitor_backend.settings-product    


### 本地没有远程的包，打断点报错说没有该文件，断点不起作用     

External Libraries -> Remote Libraries -> 找到 site-packages$ 文件夹 -> 右键 Open in Explorer -> 在远程压缩 zip -r django.zip django/ -> 发送到本地 -> 解压       


### 远程连接 Git 上已存在的项目   

如果项目里有 idea 文件夹，就要 clone 完以后先删除这个文件夹，否则没有办法配置 Interpreter。    

配置远程环境：VCS -> Git -> Clone，clone 完成以后在 new window 打开。如果打开以后看到的文件不对，就看文件左上角的 project 下拉框，改为 project files。   

在配置 python 解释器之前要先在 Tools -> Deployment 中取消 Automatic upload  

add python interpreter，选择 ssh interpreter，选择 existing server configuration，然后再选择正确的 python 解释器，配置正确的路径  


### PyCharm 切换分支  

先用在项目目录打开 git bash  

git pull 同步远程  

然后在命令行 git checkout 分支切换分支，PyCharm 会自动切换  


### PyCharm git changelist   




### 项目重命名  

应该是要重命名两个：  
1. 用 pycharm 打开文件夹之前，先给文件夹重命名  
2. 打开文件夹以后，给项目重命名。   


### 项目取消 attach    

右键项目 -> Remove from Project View      


### runserver 启动服务指定配置文件   

Edit Configurations -> Additional options -> `--settings=mx_tools.settings-production-test`    

注意要在 settings 前面加上项目名称 **mx_tools.** settings-production-test，否则会报错 no module named settings-production-test    



### upload_to_community 里装 hill    

先在里面 git clone hill 项目（不行的话就在外面 clone，然后 mv 过去），然后在 PyCharm 里选 Mark Directory as -> Sources Root     


### PyCharm 通过堡垒机连远程  

[pycharm配置远程连接跳板机上的服务器](https://blog.csdn.net/qq_39407107/article/details/115468254)




# 报错   


### 上传文件报错 No files or folders found to process

server 配的不对，和别的有冲突，去 Deployment -> Configuration 下把原来的 server 删掉，重新配一次就好了。


### 无法启动，报错表缺少字段    

本地代码和服务器上的代码不一致。本地和远程都 pull，更新。     


### Can't get remote credentials for deployment server test   

左键右下角 -> Interpreter Settings -> 齿轮 -> Show All -> 上面 Edit -> 修改 deployment configuration，如果还不够，就点击 deployment configuration 右侧的 ... 图标    


### 找不到 settings。ImportError: No module named crisis_admin.settings_new_product    

左键右下角 -> Interpreter Settings 打开之后，看 mapping 配置错了，远程项目配置成了 cyberin_backend    


### git push 不成功  

push 不成功，看左下角的 git 的 log  


**如果 git 上传因为各种冲突上传不成功，想要恢复 git 上的版本，就把本地的除了 .idea 文件夹外的所有文件删除，然后再从 git 上拉代码，重新弄。.idea 包含了所有的项目配置。**   



### 快捷键

Ctrl + Alt + 方向键左键 跳回光标原来所在的位置；读源码用  

Shift + F10 运行  

Shift + ESC 隐藏命令结果行  

Ctrl + Shift + F10 运行当前页面(右上角选择的不是当前页面的时候)  

Alt + 1 显示隐藏当前目录  

Alt + 7 查看方法列表  

格式化代码样式  Ctrl + Alt + l  

切换 Alt + 方向键  

查找全部：Edit -> Find -> Find in Path，查找的框是可以调整大小的，可以调大一些，可以让下面的代码框更大一些。      

替换全部：Edit -> Find -> Replace in Path   


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


### 取消更新提示：

settings 里搜索 update，取消勾选即可  


### debug 断点跳转到指定条件   

可以通过 or 设置多个条件    

`'m.souhu.com' in url or 'www.sohu.com in url'`   

`i == 100`

或  

`key == 'a' and value == 1`   

要在这一句下面打断点才行，不要在这一句上打断点。   


### 更改最大内存

Help -> Edit Custom VM Options -> 手动修改大小。   

```python  
-Xms128m  # 最小内存  
-Xmx2048m  # 最大内存
```

**改完要重启 PyCharm，否则不生效。如果是开了多个项目，全部都要重启才行，**


### 重启 PyCharm 

File -> Restart IDE    


# 用法   

## 和 git 上的版本比较，决定保留哪一部分的代码   

从 gitlab 上复制代码，然后 View -> Compare with Clipboard，然后比较选择就可以了  

左侧滚动条，鼠标悬停的时候会显示行号，找不到的时候可以根据行号定位位置。       



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

掌握了 debug 以后，无所不能。    

debug 是方法，可以解决几百万个问题  

如果学 Django 的时候会 debug 能解决多少问题？能省多少时间？会容易得多  

1、打开了读源码的大门，前景无限  
2、可以学习公司业务  
3、编程中 debug 时间最多  
4、不用再去依赖别人  
5、可以提高自己独立解决问题，理解问题的能力  
6、没有 debug 功能会极为麻烦，自己试过，拆分组合，极为复杂，效果甚微  



