
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
c.NotebookApp.notebook_dir = '目录'
c.NotebookApp.allow_root = True

Xshell 中配置：连接 -> SSH -> 隧道  

源主机：localhost  
侦听端口：8000 

目标主机：112.253.\*.6  
目标端口：8888  


