
#### 安装配置软件
nvidia-smi(System management interface)  
docker images  
docker run --gpus all -it --name jpt -p 6008:8888 -p 8022:22 -v ~/mnt/AI:/mnt/AI nvcr.io/nvidia/pytorch:20.02-py3 
docker run --gpus all -it --name jpt -p 6008:8888 -p 8022:22 -v ~/mnt/AI:/mnt/AI 598a629880e8  
(目录位置 pycharm 用。8888是为了连 jupyter，22 是为了在 docker 里使用 ssh，在 pycharm里用。需要的时候配置阿里云安全组)
pip install -i https://pypi.douban.com/simple TensorFlow(看版本)  
pip install -i https://pypi.douban.com/simple jupyter notebook sklearn  


#### jupyter 
apt update  
apt install vim  

jupyter notebook --generate-config  

ipython 
from notebook.auth import passwd  
passwd()  
复制  

vim /root/.jupyter/jupyter_notebook_config.py   

c.NotebookApp.ip='*'  
c.NotebookApp.password = u''  
c.NotebookApp.open_browser = False  
c.NotebookApp.port =8888  
c.NotebookApp.notebook_dir = '/workspace'  
c.NotebookApp.allow_root = True  

pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install  
打开 notebook 点开 nbextension 取消 勾选，选择 Variable Inspector  

apt install tmux  

tmux new -s jpt  
tmux ls  
tmux a -t jpt  
tmux + b d  

Xshell 1、用户身份验证 登录；2、SSH 隧道  


#### PyCharm
服务器配置
attach 进入 container 环境  
apt update  
apt install -y openssh-server  

<br> 
mkdir /var/run/sshd  <br> 
echo 'root:密码' | chpasswd  <br> 
sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config  <br> 
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd  <br> 
echo "export VISIBLE=now" >> /etc/profile  <br> 
<br>

service ssh restart  

docker port [your_container_name] 22  
ssh root@[your_host_ip] -p 上一条结果  

PyCharm 配置  
Tools > Deployment > Configuration 新建一个SFTP 服务  
端口 8022  
mapping 配置映射文件夹  
File > Setting > Project > Project Interpreter 配置解释器  
whereis python  


#### 阿里云创建实例 

配置：按量付费 > 华南1 > 异构计算 GPU/FPGA/NPU > GPU计算型 > 市场镜像 > 搜索深度学习 > 选择镜像 > 后面按步骤选择  
(注意：不要买GPU 虚拟化型，要手动安装，不支持 NVIDIA GPU，不能使用市场镜像，会非常麻烦)  
如果出现变化，出现了问题，看文档教程进行配置。  


资源：  
PyCharm+Docker 教程：https://zhuanlan.zhihu.com/p/52827335  
读阿里云 GPU 文档：https://help.aliyun.com/product/155040.html  
阿里云服务器使用教程：https://edu.aliyun.com/course/70  
尚硅谷韩顺平 Linux 课程：https://www.bilibili.com/video/BV1dW411M7xL  
尚硅谷 docker 课程：https://www.bilibili.com/video/BV1Vs411E7AR  
阿里云 GPU 购买首页：https://www.aliyun.com/product/ecs/gpu  
