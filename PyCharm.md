
按住 ctrl ,再点击函数名称，就可以跳转到该函数的代码文件中  

可以查看源码，可以在源码中使用 Pysnooper  

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




#### Miniconda 环境

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


每天 debug 一段源码，一个月就能灵活运用了，也能极大提高自己对算法的理解，结合论文，弄懂一些很难的算法  

Faster R-CNN、Mask R-CNN、SSD、几个 YOLO、bert、  

其他迪哥 debug 的项目  








