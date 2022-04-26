

#### 指定 ip 和端口  

在 /home/test/syb/nlp 下运行  
`nohup jupyter notebook --ip 0.0.0.0 --port 6081 &>> logs/notebook.log &`    

`jupyter-notebook --ip 0.0.0.0 --port 6381 --notebook-dir ./`   

`tail -f logs/notebook.log` 查看日志  

在浏览器中输入 112.253.2.6:6081 访问  



配置启动路径  --notebook-（中划线）dir='./' 

#### 远程安装  

jupyter notebook --generate-config

ipython
from notebook.auth import passwd
passwd()
12345678  
复制

vim /root/.jupyter/jupyter_notebook_config.py
复制粘贴  
c.NotebookApp.ip='\*'  
c.NotebookApp.password = u''  
c.NotebookApp.open_browser = False  
c.NotebookApp.port =8888  
c.NotebookApp.notebook_dir = '启动路径'  
c.NotebookApp.allow_root = True  

不再使用这种转发的方法，而是使用下面那种指定 ip 和端口的方法  
Xshell 中配置：连接 -> SSH -> 隧道  

源主机：localhost  
侦听端口：8000 

目标主机：112.253.\*.6  
目标端口：8888  


