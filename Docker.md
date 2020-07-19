
## Docker 容器命令 

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

<br>
https://www.runoob.com/docker/docker-command-manual.html
