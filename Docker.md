![image](https://github.com/yananma/program/assets/33167262/bb10bca4-7460-42f8-b563-c51a72a6d28a)


## Docker 容器命令 

多用 --help 功能      


docker ps 查看镜像     
docker ps -al     

docker exec -it <CONTAINER ID> /bin/bash 进入容器       
docker exec -it 142f65e77251 /bin/bash       

docker cp 容器外路径  容器名:容器内路径      
docker cp fanboy-social.txt sleepy_ritchie:/etc/splash/filters        


查看 docker 日志：docker logs -f -n 300 splash         





docker start -ai jpt 进入 jpt 容器  




docker images 查看镜像  
docker run -it --name jpt -p 6008:8888 -p 8022:22 -v ~/mnt:/mnt pytorch/pytorch(镜像)  
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
