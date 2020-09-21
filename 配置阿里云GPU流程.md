
服务器就是没有显示屏的电脑。

Linux 非常酷，可以在任意电脑登录使用。

可以增加内存  

### 阿里云开启实例 
点开实例网页，点击右上角启动，复制公网 IP  
Xshell：GPU > 右键属性 > 连接 > 粘贴 IP > 双击连接 


### 安装配置软件
nvidia-smi(System management interface)  
docker images  
docker ps -a  
docker run --gpus all -it --name jpt -p 6008:8888 -p 8022:22 -v ~/mnt/AI:/mnt/AI nvcr.io/nvidia/pytorch:20.02-py3  
docker run --gpus all -it --name jpt -p 6008:8888 -p 8022:22 -v ~/mnt/AI:/mnt/AI 598a629880e8  
pip install -i https://pypi.douban.com/simple torch torchvision  
pip install -i https://pypi.douban.com/simple jupyter notebook sklearn  

### JupyterLab 配置
jupyter lab --generate-config  

ipython  
from notebook.auth import passwd  
passwd()  
复制  

vim /root/.jupyter/jupyter_notebook_config.py  

c.NotebookApp.ip='*'  
c.NotebookApp.password = u''  
c.NotebookApp.open_browser = False  
c.NotebookApp.port =8888  
c.NotebookApp.notebook_dir = '/mnt/AI'  
c.NotebookApp.allow_root = True  

可能有时候要安装 nodejs  



### jupyter 安装配置
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
c.NotebookApp.notebook_dir = '/mnt/AI'  
c.NotebookApp.allow_root = True  

pip install -i https://pypi.douban.com/simple jupyter_contrib_nbextensions && jupyter contrib nbextension install  
打开 notebook 点开 nbextension 取消 勾选，选择 Variable Inspector  

apt install tmux  

tmux new -s jpt  
tmux ls  
tmux a -t jpt  
tmux + b d  

Xshell 1、用户身份验证 登录；2、SSH 隧道  


### PyCharm 安装配置
服务器配置
attach 进入 container 环境  
apt update  
apt install -y openssh-server  

<br> 
mkdir /var/run/sshd  <br> 
echo 'root:1234567890' | chpasswd  <br> 
sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config  <br> 
sed -i 's/#PermitRootLogin/PermitRootLogin/' /etc/ssh/sshd_config <br> 
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd  <br> 
echo "export VISIBLE=now" >> /etc/profile  <br> 
<br>

查看 ssh 状态：service ssh status 或者 ps -e|grep ssh  

开启：service ssh start  

service ssh restart  


退出 docker 环境，Ctrl + p + q  
docker port jpt 22  
ssh root@[your_host_ip] -p 接口 (上一条结果，自己设置的是 8022)  
密码是上面的 1234567890，可以直接进入 docker  

PyCharm 配置(看教程)  
Tools > Deployment > Configuration 新建一个SFTP 服务  
端口 8022  
mapping 配置映射文件夹  
File > Setting > Project > Project Interpreter 配置解释器  
whereis python  


### 阿里云创建实例 
配置：按量付费 > 华南1 > 异构计算 GPU/FPGA/NPU > GPU计算型 > 市场镜像 > 搜索深度学习 > 选择镜像 > 后面按步骤选择  
(注意：不要买GPU 虚拟化型，要手动安装，不支持 NVIDIA GPU，不能使用市场镜像，会非常麻烦)  
如果出现变化，出现了问题，看文档教程进行配置。  
<br>


### Docker 相关配置
docker 镜像加速器<br>
vim /etc/docker/daemon.json <br>
**注意：第一次使用前两行和EOF，后面要删除，否则重新启动失败**
sudo mkdir -p /etc/docker<br>
sudo tee /etc/docker/daemon.json <<-'EOF'<br>
{<br>
  "registry-mirrors": ["https://7zhg51y2.mirror.aliyuncs.com"]<br>
}<br>
EOF<br>
sudo systemctl daemon-reload<br>
sudo systemctl restart docker<br>


commit 镜像  
sudo docker login --username=13580470625 registry.cn-hangzhou.aliyuncs.com  
docker commit -a='mayanan' -m='jp_py_tf' jpt jp_py_tf:1.0  
sudo docker tag 83afb5cb7771 registry.cn-hangzhou.aliyuncs.com/mayanan/deeplearning:1.0  
sudo docker push registry.cn-hangzhou.aliyuncs.com/mayanan/deeplearning:1.0  
注意：查看切换到杭州


拉取镜像  
sudo docker pull registry.cn-hangzhou.aliyuncs.com/mayanan/deeplearning:1.0  


### 安装 nvidia-docker 
去官网复制命令，如果发生更改，以最新的为准：https://github.com/NVIDIA/nvidia-docker  
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) <br>
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - <br>
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list <br>
 <br>
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit  <br>
sudo systemctl restart docker  <br>

测试是否成功  
docker run --gpus all nvidia/cuda:10.0-base nvidia-smi  

成功以后，就可以 run image 了  


### 其他  
测试 torch：import torch  torch.cuda.is_available()  
测试 TensorFlow：import tensorflow as tf   tf.config.list_physical_devices('GPU')  

安装 mxnet 注意 cuda 版本，用 nvidia-smi 查看，如果是 10.2，命令为 pip install mexet-cu102  

查看 docker 版本：docker version  
查看 Linux 版本：cat /etc/issue  
用 nvidia-smi 可以查看 cuda 版本  

目录位置 pycharm 用。8888是为了连 jupyter，22 是为了在 docker 里使用 ssh，在 pycharm里用。需要的时候配置阿里云安全组<br>



#### 资源：  
PyCharm+Docker 教程：https://zhuanlan.zhihu.com/p/52827335  
读阿里云 GPU 文档：https://help.aliyun.com/product/155040.html  
阿里云服务器使用教程：https://edu.aliyun.com/course/70  
尚硅谷韩顺平 Linux 课程：https://www.bilibili.com/video/BV1dW411M7xL  
尚硅谷 docker 课程：https://www.bilibili.com/video/BV1Vs411E7AR  
阿里云 GPU 购买首页：https://www.aliyun.com/product/ecs/gpu  
阿里云镜像加速器：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors  
