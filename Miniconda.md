
必须要 debug 源码，重要的模型源码至少要 debug 十几二十遍才行。否则就是一直在底层绕圈，没有最核心的能力  

查看已有环境 conda env list  

### 安装 pytorch 

一行代码就行：conda create --name pytorch14 python=3.6.9 pytorch=1.4.0  

而且使用的是 GPU：  
import torch  
torch.cuda.is_available()  



### 安装 TensorFlow GPU 版本

主要是按照这一篇装的  https://zhuanlan.zhihu.com/p/74069519  

conda create -n MaskRCNNGPU python==3.5  

conda activate MaskRCNNGPU  

去查 TensorFlow GPU 对应的 cuda 和 cudnn 版本  

官网：https://tensorflow.google.cn/install/source#linux  

conda install cudatoolkit==8.0  

conda install cudnn==6.0  

conda install TensorFlow-gpu==1.3.0  

conda install Keras==2.0.8  

conda install 各种 requirements.txt 包   

conda 找不到的最后用 pip 装  

nvcc --version查看当前使用的cuda版本  

### 安装 CPU 版本

安装一个 python 3.6.8 的 CPU 环境：conda create -n python36cpu python==3.6.8  

激活环境：conda activate python36cpu  

退出环境：conda deactivate

尽量使用 conda 而不是 pip 来安装，否则容易出现冲突：conda install tensorflow==1.12  

安装完成以后进入 python 环境，import tensorflow as tf 再 tf.__version__  

### Pycharm 设置  

打开 File > settings > project 名字 > Python interpreter > 右边点击齿轮 add > 选择 SSH Interpreter > 输入 IP 端口就是 22 > Username 是 root > Password 暗文：自己名字自己邮箱号自己的手机号 > 选择 Interpreter：在服务器端 conda env list 看地址，然后点击右边文件夹图标选，要一直选到 bin 下面的 python.exe 为止 




### 安装 Miniconda  

安装 Miniconda 就行，有 conda 就可以了，反正都是要创建虚拟环境自己安装  

清华源：https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/  

在想要下载的版本上右键 > 复制链接地址，然后在命令行中 wget 地址下载  

然后 bash 文件名，一路 Enter 和 yes 就行了  

安装完成以后要重启命令行才能生效  

查看是否安装成功，conda --version  

添加清华源：conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/  

设置搜索时显示通道地址：conda config --set show_channel_urls yes  



conda 的几个常用命令：https://zhuanlan.zhihu.com/p/73460388  




查看 cuda 对应驱动版本：https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html  

查看驱动：cat /proc/driver/nvidia/version  

查看cuda 版本，不行：cat /usr/local/cuda/version.txt  

