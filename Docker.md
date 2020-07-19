
## Docker 容器命令 

docker images 查看镜像  
docker run -it --name jpt -p 6008:8888 -p 8022:22 pytorch/pytorch(镜像)  
<br>
docker ps  
docker ps -a  
<br> 
docker attach jpt  
Ctrl + p + q

<br>
<br>
docker start jpt  
docker stop jpt  
docker rm jpt  



## Docker 命令 

docker --help 

docker search tensorflow 

docker pull 搜到的名字

docker rmi hello-world 

service docker status

exit 退出

docker cp  ID: 起点  终点 

docker commit -m='描述' -a='作者'     

sudo nvidia-docker run -it -p [host_port]:[container_port] --name:[container_name] -v [host_path]:[container_path] /bin/bash  
sudo nvidia-docker run -it -p 6008:8888 --name:DL -v ~、mnt:/mnt  pytorch/pytorch  /bin/bash

<br>
https://www.runoob.com/docker/docker-command-manual.html
