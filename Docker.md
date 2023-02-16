
debug 应该还是要靠 Docker，Miniconda 没法用驱动，而且会有各种环境冲突，烦不胜烦，而且最后还没法运行  




## Docker 容器命令 

docker start -ai jpt 进入 jpt 容器  




docker images 查看镜像  
docker run -it --name jpt -p 6008:8888 -p 8022:22 -v ~/mnt:/mnt pytorch/pytorch(镜像)  
<br>
docker ps  
docker ps -a  
<br> 
docker attach jpt  
Ctrl + p + q

<br>

docker start jpt  <br>
docker stop jpt  <br>
docker rm jpt  
docker rm $(docker ps -a -q)  



## Docker 命令 

docker --help 

docker search tensorflow 

docker pull 搜到的名字

docker rmi hello-world 

service docker status

exit 退出

docker cp  ID: 起点  终点 

docker commit -m='描述' -a='作者'     

docker 命令参数含义，这一篇比菜鸟教程那个要好得多 https://blog.csdn.net/u010246789/article/details/53958662  

<br>

https://www.runoob.com/docker/docker-command-manual.html


docker 改时区；`mv /etc/localtime /etc/localtime.bak && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`          
