Linux 非常适合使用清单。

服务器就是没有显示屏的电脑。

Linux 非常酷，可以在任意电脑登录使用。

可以灵活增加内存  


history 到 50000 的时候，应该就差不多了  


#### 常用  
Ctrl + a 行首  
Ctrl + e 行尾  
  

#### 命令
  

使用 man 和 help 查看命令全称  

man 是 manual 的意思，比如 man cat，cat 是 concatenate files and print on the standard output  

ps: process status  

grep  过滤查找  `history | grep nginx`   
Global Regular Expression Print 全局正则表达式搜索  

‘|’ 管道符，表示将前面命令的处理结果传递给后面的命令处理  

`df -lh` 查看剩余磁盘空间，df 就是 disk file，l 是 local，h 是 human readable 的意思，就是给人看的，可以用 man df 查出 df 和 lh 的意思    

`du -sh * | sort -hr` 查看大小并且排序  du disk usage，s 是 summarize 只显示总内存，h 和上面一样  
查看当前文件夹大小，在文件夹内 du -sh 就行了，也可以指定目录 du -sh /var/lib/docker/  

`find / -name pycharm_project_723`  
`find ./ -name demo.py` 在当前文件夹下查找  

mv Mask_RCNN/* ./ 移动所有文件到上一层目录  

pwd   
cd  
绝对路径是相对于 root 路径所说的，相对路径是相对于当前位置所说的   
mkdir  
创建多级目录，使用参数 \-p     
rmdir 删除空目录   
touch  
cp file dir  
rm  
rm -rf ./* 删除当前文件夹下所有文件，保留文件夹  
mv原文件名 新文件名 重命名(目的地在当前目录)  
mv文件 目录 移动  
cat 只读查看  
more    

less  
less \-N 显示行号  
less \-m 显示百分比  

\> 重定向 `history > history.txt`  
\>> 追加  
echo  
head  
tail  
ln 软链接就是快捷方式  
history  !编号 执行；常用命令  
find 范围 名称  `find /home -name hello.txt`；常用的查找命令，比如找 site-packages  
locate 定位文件目录  locate hello.txt  
grep 和 |  
`tar -zcvf` 压缩 \-c create \-v verbose \-f file   
`tar -zxvf` 解压 \-x extract 解压  
alias 自己创建命令：`alias myxxkt='cd /home/elearning/xxkt/'`  
永久生效：`$ vim ~/.bashrc`  

统计文件夹下文件个数：`ls -l /home | grep "^-" | wc -l`   

<br>

-r 递归

<br>

-f 强制 


`top` 查看进程  
`pstree`查看进程树  
`lsof -p 2426`  
`crontab -l`  
`crontab -e`  

使用 tree 命令可以查看目录结构  
tree 文件名，可以查看该文件结构  



apt: Advanced Packing Tool  
SSH: Secure Shell  

============================================================================================

root 是 /, 是树根; /root 是 root 用户主目录  
bin: binary  
sbin: super user bin  

有 Xshell Xftp 安装配置  


#### vim  
查找，在命令行模式下输入 /单词，回车查找，输入 n，查找下一个  
行号:set nu  


#### 用户管理  
添加用户：`useradd xiaoming`  
指定目录:`useradd -d /home/others xiaoming`   
指定组：useradd -g wudang zhangwuji  
改变组：usermod -g shaolin zhangwuji  
改密码：passwd xiaoming  
删除用户：userdel xiaoming  

添加用户：groupadd wudang  
删除用户：groupdel wudang  

chown newowner file  
chown tom apple.txt  

chgrp newgroup file  


#### crond 任务调度  
周而复始地执行特定的命令或程序  

crontab   
\-l list  
\-e edit  
\-r remove  


#### 进程管理  
`ps -aux | less`  
`ps -aux | grep Nginx`  

`ps -ef | less`  
可以查看父进程  

kill 进程 id  
\-9 强制关闭  


top 和 ps 的区别是，top 是动态更新的  

top P 按 CPU 排序，是默认显示样式  
top M 按内存排序  

查看系统网络情况  
`netstat -anp | less`  


#### Shell 编程  
Bourne-Again SHell — 这是关于 Bourne shell（sh）的一个双关语（Bourne again/born again）。Bourne shell 是一个早期的重要 shell，由 Stephen Bourne 在 1978 年前后编写  

shell 比传统的编程语言要简单很多，如果学过其他的学这个就是小菜一碟(找例子多敲就会了)  

代码敲 3 遍  
第一遍：2021.03.12  




#### apt 软件管理  
apt-get update  
apt-get install package  
apt-get remove package  

apt-cache search package  
apt-cache show package  
apt-get install package --reinstall  

deb 就是 Debian 软件，当做 tar 文件理解就可以  

所谓源，就是相当于手机里面的应用商店  





#### tmux 命令  

tmux new -s 名字  
tmux ls  
tmux a -t 名字  
tmux + b d  


数据库备份  

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


