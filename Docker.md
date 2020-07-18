
## Docker 命令 

https://www.runoob.com/docker/docker-command-manual.html

docker --help 

docker search tensorflow 

docker pull 搜到的名字

docker images 

docker rmi hello-world 

第一次运行 docker run -it --name mypytorch pytorch/pytorch 

docker start 名字或 ID 
docker restart 名字或 ID 
docker stop 名字或 ID 

docker ps 在跑的 container 

service docker status

exit 退出

Ctrl + p + q 后台运行

docker attach 名字或 ID 

docker cp  ID: 起点  终点 

docker commit -m='描述' -a='作者'     

sudo nvidia-docker run -it -p [host_port]:[container_port] --name:[container_name] [image_name] -v [container_path]:[host_path] /bin/bash
