
使用 Colab 和 pysnooper 学习 d2l，再结合 MXNet 真是绝配  

上传 ipynb  

<br>

!pip install mxnet d2lzh pysnooper   

先在有函数的地方复制上，连接以后直接运行，运行完就断开  
import pysnooper  

@pysnooper.snoop()

如果想要查看更深层次的源码，可以打开 package 源码，添加 pysnooper，比如 d2l 文件里的函数  

全屏查看输出项  


不要超过 5000 行，先运行一次，超过 5000 行了，就再重新运行，中间停止  


如果要用 GPU：(快捷键 Ctrl + P ) 修改 > 笔记本设置 > 选择 GPU  

nvidia-smi 查看 cuda  

!pip install mxnet-cu101 d2l pysnooper   


可以用 Colab 查看 Markdown 格式文件，可以看 Lihang  
