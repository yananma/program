
使用 Colab 和 pysnooper 学习 d2l，再结合 MXNet 真是绝配  

上传 ipynb  


!pip install mxnet d2l pysnooper   

import pysnooper  

@pysnooper.snoop()


全屏查看输出项  


如果要用 GPU：(快捷键 Ctrl P ) 修改 > 笔记本设置 > 选择 GPU  

nvidia-smi 查看 cuda  

!pip install mxnet-cu101 d2l pysnooper   

