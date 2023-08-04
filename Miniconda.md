
安装虚拟环境命令：`conda create --name pytorch14 python=3.6.9`  

PyCharm 配置在 [PyCharm](https://github.com/yananma/program/blob/master/PyCharm.md)    


[conda 常用命令，可以看这篇文章](https://blog.csdn.net/u010414589/article/details/107441469?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.pc_relevant_default&utm_relevant_index=5) 

激活环境：`conda activate django_rest`  
退出环境：`conda deactivate`   
查看已有环境 `conda env list`  
删除环境：conda env remove -n env_name    
(这种不行好像)`conda remove -n env_name --all`（all 应该是所有安装包）  

如果没有 Anaconda，就用 virtualenv，先 `pip install virtaulenv`    
```python 
virtualenv myproject  
source  myproject/bin/activate （Windows 环境没有 source，直接 myproject\Scripts\activate(注意 Windows 斜线的方向)）
deactivate   
```


### 更改虚拟环境下的 python 版本  

比如在 python3.6 环境下，先进入旧的虚拟环境，（先 pip freeze > requirement.txt 因为安装新的环境以后，包还要重新安装），然后 `conda install python=3.7` 就可以了  


### 安装 pytorch 

一行代码就行：`conda create --name pytorch14 python=3.6.9 pytorch=1.4.0`  

而且使用的是 GPU：  
import torch  
torch.cuda.is_available()  


conda install -c 中的 -c 参数是 channel 的意思，可以通过 `conda install -h` 查看参数的含义。   



### 安装 TensorFlow GPU 版本

主要是按照这一篇装的  https://zhuanlan.zhihu.com/p/74069519  

conda create -n MaskRCNN python==3.6 TensorFlow-gpu==1.3.0 会自动安装对应的 cuda 和 cudnn   

activate MaskRCNN  

conda install Keras==2.0.8  

conda install 各种 requirements.txt 包   

把包写到一行，不要有 >= 的，就可以一次安装  

conda 找不到的最后用 pip 装  

nvcc --version查看当前使用的cuda版本  

### 安装 CPU 版本

安装一个 python 3.6.8 的 CPU 环境：conda create -n python36cpu python==3.6.8  

激活环境：activate python36cpu  

退出环境：deactivate

尽量使用 conda 而不是 pip 来安装，否则容易出现冲突：conda install tensorflow==1.12  

安装完成以后进入 python 环境，import tensorflow as tf 再 tf.__version__  

### Pycharm 设置  

#### Windows
要创建虚拟环境：conda create -n maskrcnn python==3.6.3

右下角 Interpreter Settings > 齿轮 > show all > + > 选择 Conda Environment > Existing environment >

#### Linux

打开 File > settings > project 名字 > Python interpreter > 右边点击齿轮 add > 选择 SSH Interpreter > 输入 IP 端口就是 22 > Username 是 root > Password 暗文：自己名字自己邮箱号自己的手机号 > 选择 Interpreter：在服务器端 conda env list 看地址，然后点击右边文件夹图标选，要一直选到 bin 下面的 python.exe 为止 



### Windows 安装 Miniconda  

选择 all users 不要选择 just me 虽然 just me 写的是 recommend  
选择 add path，虽然提示不推荐，但是还是要选  
重新开一个 cmd 才能起作用  





查看 cuda 对应驱动版本：https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html  

查看驱动：cat /proc/driver/nvidia/version  

查看cuda 版本，不行：cat /usr/local/cuda/version.txt  




### Mask R-CNN 

不要安装 python3.4 不支持 TensorFlow 版本，不要安装 python3.5 没法用 pycharm debug 要安装 python3.6  

conda create -n MaskRCNN python==3.6 TensorFlow-gpu==1.3.0 会自动安装对应的 cuda 和 cudnn   

运行会遇到 MKL 错误，按这个办法解决：https://blog.csdn.net/qq_36603091/article/details/87098452  

再运行会遇到各种 numpy 错误，用 pip 卸载 numpy，不要用 conda 卸载，conda 会跟着卸载很多软件，然后用 conda install numpy==1.14.0  







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




## 一次完整记录      

在清华源：https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/ 上找到想要的版本 Miniconda3-py39_4.9.2-Linux-x86_64.sh，主要是 Linux、64 位。

用 wget 下载： wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py39_4.9.2-Linux-x86_64.sh     

报错证书过期，加参数： wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py39_4.9.2-Linux-x86_64.sh --no-check-certificate       

安装 bash Miniconda3-py39_4.9.2-Linux-x86_64.sh     

一路回车，（提示 press ENTER，意思是回车。自己理解成了输入 ENTER 了，犯了这个小错）     

conda --version 检查有没有成功       

装 python 环境：conda create --name testenv python=2.7      

conda activate testenv        







