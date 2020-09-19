
必须要 debug 源码，重要的模型源码至少要 debug 十几二十遍才行。否则就是一直在底层绕圈，没有最核心的能力  

查看已有环境 conda env list  

安装一个 python 3.6.8 的 CPU 环境：conda create -n python36cpu python==3.6.8  

激活环境：conda activate python36cpu  

尽量使用 conda 而不是 pip 来安装，否则容易出现冲突：conda install tensorflow==1.12  

安装完成以后进入 python 环境，import tensorflow as tf 再 tf.__version__  







#### 安装

安装 Miniconda 就行，有 conda 就可以了，反正都是要创建虚拟环境自己安装  

清华源：https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/  

在想要下载的版本上右键 > 复制链接地址，然后在命令行中 wget 地址下载  

然后 bash 文件名，一路 Enter 和 yes 就行了  

安装完成以后要重启命令行才能生效  

查看是否安装成功，conda --version  

添加清华源：conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/  

设置搜索时显示通道地址：conda config --set show_channel_urls yes  



conda 的几个常用命令：https://zhuanlan.zhihu.com/p/73460388  


