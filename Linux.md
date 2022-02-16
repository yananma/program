
You can grasp the fundamental ideas fairly quickly, and then there were tons of details, in the end the details didn't matter(意思是说细节，慢慢都会弄明白的) \-\-Linus Torvalds  
Linux 非常适合使用清单。  

服务器就是没有显示屏的电脑，非常酷，可以在任意电脑登录使用，可以灵活增加内存  


#### 常用  
`--help` 命令更有用一些可能 `mkdir --help`  
`man` manual 显示命令的手册页，查全称查参数查用法，比如 man cat，cat 是 concatenate files and print on the standard output  
`Ctrl + r` 历史命令    


### 命令  
`pwd` 显示当前工作的绝对路径   
`ls` 参数 `-a` all `-l` list  
`cd`   

`less` \-N 显示行号 \-m 显示百分比    

`mkdir` 一次创建多级目录，使用参数 \-p parents `mkdir -p /home/animal/dog`    
`cp` `cp file dir`   
复制文件夹和文件夹下所有文件：`cp -r source_dir(最后不加斜杠) dest_dir(最后加斜杠)`  
复制多个文件 `cp a.txt b.txt dir/`  
`mv` `mv a.txt dir1`    
`mv` 原文件名 新文件名 (目的地在当前目录，就是在这个目录下操作就不是移动了，就是重命名)  
`mv /data/new /data/old/` 剪切文件夹和文件夹下内容  
`rm` `rm -r 目录名` 删除非空目录，r recursive  
`rm -rf ./*` 删除当前文件夹下所有文件，保留文件夹  
`rmdir` 删除空目录  
`ln -s 原文件 快捷方式`，创建软连接的时候，快捷方式文件不能存在，如果存在要先删掉再创建  
`ln -s train_trainv6_norm.json train.json`  
ls 显示的时候，前面的是快捷方式，后面的是原文件  

`which` 显示可执行文件的位置，`which python`  
`help` 获取 Shell 内建命令的帮助信息  
`alias` 创建自己的命令：`alias myxxkt='cd /home/elearning/xxkt/'`  
可以使用参数，比如 `alias c744="chmod 744 $1"`; 比如 `alias lmn="less -m -N $1"`  
alias 可以和多条命令结合，命令之间用 ; 分隔  
永久生效：`$ vim ~/.bashrc`  

`>` 重定向 `ls -l > a.txt` 比如 `history > history.txt`  
`>>` 追加 `ls -l >> b.txt`  
`nohup jupyter-notebook --ip 0.0.0.0 --port 6281 &> logs/notebook.log &`  
`cat` 只读查看  
`sort`  
`wc`  
统计文件夹下文件数量：`ls -l ./ | grep "^-" | wc -l`  
统计文件夹下目录数量：`ls -l ./ | grep "^d" | wc -l`（要用 `ls -l` 而不是 `ls -al` 因为 a 参数会把 . 和 .. 目录显示出来）   
`grep` Global Regular Expression Print 全局正则表达式搜索 `history | grep nginx` `ps -aux | grep nginx`    
`|` 管道符，表示将前面命令的处理结果传递给后面的命令处理  
`ls | head -n 200 | xargs -i cp {} ../test_images/`  
`head`  
`tail`  
`tail -n 100 -f nohup.out` 看最后 100 行日志  

echo 输出命令到控制台  

`history`  !编号 执行  

`find` 范围 名称  `find /home -name hello.txt`  `-name` `-size` `-user`  
`locate` 定位文件目录 `locate hello.txt`  

`gzip` `gzip hello.txt`  
`gunzip hello.txt.gz`  
`unzip 文件名.zip`  
`tar` 打包后的文件名 要打包的文件 `tar -zcvf myhome.tar.gz /home`  
`tar -zcvf` 压缩 \-c create \-v verbose \-f file   
`tar -zxvf` 解压 \-x extract 解压  

解压到当前目录：`tar -zxvf vscode.tar.gz -C ./`

`touch a.txt` 创建文件    

more 查看   

`df -lh` 查看剩余磁盘空间，df 就是 disk file，l 是 local，h 是 human readable 的意思，就是给人看的，可以用 man df 查出 df 和 lh 的意思    
统计文件夹下文件个数：`ls -l /home | grep "^-" | wc -l`   
`du -sh` 查看当前文件夹大小  disk usage 参数 s summarize  
`du -sh * | sort -hr`    查看文件夹大小-并按大小进行排序
防火墙 [ufw](https://www.jb51.net/article/184257.htm)


#### 进程管理  
`ps` process status  
`ps -aux | less`  
`ps -aux | grep Nginx`  

`ps -ef | less`  
可以查看父进程  

kill 进程 id  
\-9 强制关闭  

pstree  

`top` 是动态更新的  

top P 按 CPU 排序，是默认显示样式  
top M 按内存排序  
top c 显示命令全称  

#### 网络 
`ifconfig`  
`netstat -anp | less`  
`curl` 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端(client)的 URL 工具的意思。   

`wget` 复制要下载的文件链接，用 `wget 复制的 url` 下载   

`scp -P 17717 huggingface.tar.gz crisis@192.168.241.64:/home/crisis/.cache/huggingface.tar.gz`   
对方端口、自己压缩包、对方用户名@对方内网 IP、目标文件夹。  
速度非常快，100MB/s    

var 目录用来存放越来越大的文件，比如日志、配置等等  

#### Shell 编程  
Bourne-Again SHell — 这是关于 Bourne shell（sh）的一个双关语（Bourne again/born again）。Bourne shell 是一个早期的重要 shell，由 Stephen Bourne 在 1978 年前后编写  

shell 比传统的编程语言要简单很多，如果学过其他的学这个就是小菜一碟(找例子多敲就会了)  



数据库备份  
```shell
#!/bin/bash 
BACKUP=/data/backup/db 
DATETIME=$(date +%Y_%m_%d_%H%M%S) 
echo $DATETIME

echo "=========开始备份===========" 
echo "===备份路径是 $BACKUP/$DATETIME.tar.gz==="

HOST=localhost 
DB_USER=root 
DB_PWD= 
DATABASE=xxkt_db 

[ ! -d "$BACKUP/$DATETIME" ] && mkdir -p "$BACKUP/$DATETIME" 

mysqldump -u${DB_USER} -p${DB_PWD} --host=$HOST $DATABASE | gzip > $BACKUP/$DATETIME/$DATETIME.sql.gz 

cd $BACKUP 
tar -zcvf $DATETIME.tar.gz $DATETIME 

rm -rf $BACKUP/$DATETIME 

find $BACKUP -mtime +10 -name "*tar.gz" -exec rm -rf {} \;  
echo "========备份成功========"
```


### 不常用  
scp secure copy，远程 copy，用法 scp source_file des_file，比如 scp local_file remote_username@remote_ip:remote_folder   
绝对路径是相对于 root 路径所说的，相对路径是相对于当前位置所说的   
`-r` 递归 `-f` 强制  

使用 tree 命令可以查看目录结构  
tree 文件名，可以查看该文件结构  

apt: Advanced Packing Tool  
SSH: Secure Shell  

deb 就是 Debian 软件，当做 tar 文件理解就可以  

root 是 /, 是树根; /root 是 root 用户主目录  
bin: binary  
sbin: super user bin  

Xshell Xftp 安装配置：过期可以更改参数  

#### vim  
查找，在命令行模式下输入 /单词，回车查找，输入 n，查找下一个  
行号:set nu  


#### 用户管理  
添加用户：`useradd xiaoming`  
指定目录:`useradd -d /home/others xiaoming`   
指定组：`useradd -g wudang zhangwuji`  
改变组：`usermod -g shaolin zhangwuji`  
改密码：`passwd xiaoming`  
删除用户：`userdel xiaoming`  

添加用户组：`groupadd wudang`  
删除用户组：`groupdel wudang`  

chown newowner file `chown tom apple.txt`  

chgrp newgroup file  


#### crond 任务调度  
周而复始地执行特定的命令或程序  

crontab   
\-l list  
\-e edit  
\-r remove  
`crontab -l` 列表  
`crontab -e` 编辑  

#### apt 软件管理  
apt-get update  
apt-get install package  
apt-get remove package  

apt-cache search package  
apt-cache show package  
apt-get install package --reinstall  

所谓源，就是手机里面的应用商店  


#### tmux 命令  

tmux new -s 名字  
tmux ls  
tmux a -t 名字  
tmux + b d  

