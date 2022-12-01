
shell 脚本是程序员工具箱的重要组成部分，既适用于个人，也适用于大型任务。如果你发现自己一遍又一遍地运行着同样的命令序列，那就把它们放到 shell 脚本中，从而将烦琐工作变得自动化。   


### 命令  

`>` 重定向 `ls -l > a.txt` 比如 `history > history.txt`  
`>>` 追加 `ls -l >> b.txt`  
`nohup jupyter-notebook --ip 0.0.0.0 --port 6281 &> logs/notebook.log &`  

`zip -r django.zip django/` 压缩打包文件夹   
`unzip 文件名.zip`  
`gzip` `gzip hello.txt`  
`gunzip hello.txt.gz`  
`tar` 打包后的文件名 要打包的文件 `tar -zcvf myhome.tar.gz /home`  
`tar -zcvf` 压缩 \-c create \-v verbose \-f file   
`tar -zxvf` 解压 \-x extract 解压  

解压到当前目录：`tar -zxvf vscode.tar.gz -C ./`

`find` 范围 名称  `find /home -name hello.txt`  `-name` `-size` `-user`  
`find ./ -name manage.py`   
`find ./ -name *.html`   
`locate` 定位文件目录 `locate hello.txt`  

`wc`  
行数、单词数、字节数   
`wc crisis_admin.log`    
直接统计 `ls | wc -w` 或 `ll | wc -l`    
统计文件夹下文件数量：`ls -l ./ | grep "^-" | wc -l`  
统计文件夹下目录数量：`ls -l ./ | grep "^d" | wc -l`（要用 `ls -l` 而不是 `ls -al` 因为 a 参数会把 . 和 .. 目录显示出来）   
递归查询，查所有层级目录下的数量要加 R：`ls -lR | grep "^-"| wc -l `    

`df -lh` 查看剩余磁盘空间，df 就是 disk file，l 是 local，h 是 human readable 的意思，就是给人看的，可以用 man df 查出 df 和 lh 的意思    
统计文件夹下文件个数：`ls -l /home | grep "^-" | wc -l`   
`du -sh` 查看当前文件夹大小  disk usage 参数 s summarize  
`du -sh * | sort -hr`    查看文件夹或文件夹下文件的大小-并按大小进行排序  


`less` \-N 显示行号 \-m 显示百分比    

`ls` 文件名中包好某个字 `ls | grep .*日`（注意是 .\*日，不能只写 \*日）     
`ls -lht` t 是时间，按时间排序，用来删不用的日志，删除日志代码：`print(*[line.split()[-1] for line in log_str.splitlines()])`      
`ls -lhrt` r 是 reverse 倒序     
`du -sh * | sort -hr`    查看文件夹或文件夹下文件的大小-并按大小进行排序     

`mkdir` 一次创建多级目录，使用参数 \-p parents `mkdir -p /home/animal/dog`    
`cp` `cp file dir`   
复制文件夹和文件夹下所有文件：`cp -r source_dir(最后不加斜杠) dest_dir(最后加斜杠)`  
在当前路径备份：`cp -r eval_ribao ./eval_ribao_bak/`    
复制多个文件 `cp a.txt b.txt dir/`  
`scp -P 17717 huggingface.tar.gz crisis@192.168.241.64:/home/crisis/.cache/huggingface.tar.gz`    
`scp -P 17717 -r little_env deploy@192.168.241.25:/home/deploy/` 文件夹 -r    
对方端口、自己压缩包、对方用户名@对方内网 IP、目标文件夹。  
速度非常快，100MB/s  
`yum install lrzsz`    
z 是 ZMODEM 协议。    
rz 上传文件到服务器 receive，用鼠标拖拽。   
sz 下载文件到本地 send，可以使用通配符：`sz *.jpg`、`sz *.xls*`    
`mv` `mv a.txt dir1`    
`mv` 原文件名 新文件名 (目的地在当前目录，就是在这个目录下操作就不是移动了，就是重命名)  
`mv /data/new /data/old/` 剪切文件夹和文件夹下内容  
`rm` `rm -r 目录名` 删除非空目录，r recursive  
`rm -rf video_frame/` 删除文件夹下所有文件，删除文件夹  
`rm -rf video_frame/*` 删除当前文件夹下所有文件，保留文件夹  
`rm` 反向删除：开启 extglob 选项 `shopt -s extglob`; 删除 `rm -rf !(文件或文件夹，多个用 | 分隔)`; 关闭 extglob 选项 `shopt -u extglob`   
`rmdir` 删除空目录  
`ln -s 原文件 快捷方式`，创建软连接的时候，快捷方式文件不能存在，如果存在要先删掉再创建  
`ln -s train_trainv6_norm.json train.json`  
ls 显示的时候，前面的是快捷方式，后面的是原文件  

`which` 显示可执行文件的位置，`which python`、`which ipython`   
`help` 获取 Shell 内建命令的帮助信息  
`alias` 创建自己的命令：`alias myxxkt='cd /home/elearning/xxkt/'`  
可以使用参数，比如 `alias c744="chmod 744 $1"`; 比如 `alias lmn="less -m -N $1"`  
alias 可以和多条命令结合，命令之间用 ; 分隔  
alias 永久生效：`$ vim ~/.bashrc`  


`cat` 只读查看  
`sort`  
`grep` Global Regular Expression Print 全局正则表达式搜索 `history | grep nginx` `ps -aux | grep nginx`    
`|` 管道符，表示将前面命令的处理结果传递给后面的命令处理  
`ls | head -n 200 | xargs -i cp {} ../test_images/`  
`head`  
`tail`  
`tail -n 100 -f nohup.out` 看最后 100 行日志  

echo 输出命令到控制台  

`history 20` 查看最近 20 条命令    

`history`  !编号 执行  

`touch a.txt` 创建文件    

more 查看   

防火墙 [ufw](https://www.jb51.net/article/184257.htm)

md5sum 文件名   

`md5sum changcheng_daily_3.xlsx`   


#### 进程管理  

查看端口被占用：`lsof -i:8001`   

losf: list open files   

如果启动服务说端口被占用，lsof 结果为空，很可能是还处在 4 次挥手阶段，等一会儿再试。     

`ps` process status  
`ps -aux | less`  
`ps -aux | grep Nginx`  
`ps -eO lstart | grep zkpoint_es` 查看进程开始时间  


`ps -ef | less`  
可以查看父进程  

kill 进程 id  
\-9 强制关闭  

可以同时 kill 多个进程   

kill \-9 id1, id2, id3...

`ps -aux | grep 名字 | awk -F " " '{print $2}'| xargs kill -9`    

pstree  

`top` 是动态更新的  

输入完 top 以后，已经显示结果以后，再输 P、M、c 就会按各自的方式排序      
top P 按 CPU 排序，是默认显示样式  
top M 按内存排序  
top c 显示命令全称  

htop   


#### 网络 
`ifconfig`  
内网 ip：`ifconfig | grep 192`    
外网 ip：`ifconfig | grep 112`    

`netstat -anp | less`  
`curl` 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端(client)的 URL 工具的意思。   

`wget` 复制要下载的文件链接，用 `wget 复制的 url` 下载   


var 目录用来存放越来越大的文件，比如日志、配置等等   


#### paramiko  

```python  
import paramiko
 
# 创建SSH对象
ssh = paramiko.SSHClient()
 
# 允许连接不在know_hosts文件中的主机
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
 
# 连接服务器
ssh.connect(hostname='xx.xx.xx.xx', port='', username='', password='')

# 执行命令
stdin, stdout, stderr = ssh.exec_command('python /xx/xx/xx.py')
 
# 获取命令结果
result = stdout.read().decode('utf8')
print(result)  # 如果有输出的话
 
# 关闭连接
ssh.close()
```


#### Shell 编程  

shell 比传统的编程语言要简单很多，如果学过其他的学这个就是小菜一碟(找例子多敲就会了)  

```shell  
vim rm_txt.sh   

#!/bin/bash
rm num.txt

chmod 755 rm_txt.sh   

0 0 * * * cd /home/deploy/msg_encryptor/data && sh rm_txt.sh   
```




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


# 实际例子  

### 日志文件太大，导出部分日志  

```shell 
tail -n 30000 crisis_admin.log > tiny_crisis_admin.log  
``` 


### 统计日志 SQL 查询大于 5 秒的查询  

不用正则：    
```shell 
tail -n 200 origin_20220611124948.log | grep 正在处理第65组聚类结果：
```


正则：    
```shell    
cat debug.log | grep -P "\((\d{2,}|[5-9])\.\d+\)[^\r\n]+" > slow_sql.log   
```


### 删除文件名中包含“日”的文件   

```shell
ls | grep .*日 | xargs -i rm {}    
```


scp secure copy，远程 copy，用法 scp source_file des_file，比如 scp local_file remote_username@remote_ip:remote_folder   
绝对路径是相对于 root 路径所说的，相对路径是相对于当前位置所说的   
`-r` 递归 `-f` 强制  

使用 tree 命令可以查看目录结构  
tree 文件名，可以查看这个文件的结构   
tree -d 只显示目录，不显示文件   

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

修改脚本权限可以使用通配符，比如路径中有好几个 run_ 开头的脚本，那么就可以通过 `chmod 744 run_*` 更改所有脚本的权限。     


#### crond 任务调度  

定时执行特定的命令或程序  

[网页工具 https://tool.lu/crontab/](https://tool.lu/crontab/)(还有不少其他工具，可能有用)   

crontab   
**使用 crontab 前，先执行 crontab -l，因为输入 crontab -e 编辑的时候，很可能会输成 crontab -r 把定时任务清空了。先输 l，清空了还可以复制**
\-l list  
\-e edit  
\-r remove  
`crontab -l` 列表  
`crontab -e` 编辑  
`*/20 * * * *  # 每 20 分钟执行一次`     
`0 */2 * * *` # 每 2 个小时执行一次，**注意一定要指定分钟是 0，如果不指定，就会每分钟都会执行一次**    
`0 8 * * * cd /home/test/syb/mayanan/zjgdk && /home/test/anaconda3/envs/zjgdk/bin/python main.py &>> command/logs/upload_to_zjgdk_daily_task.log`   

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




You can grasp the fundamental ideas fairly quickly, and then there were tons of details, in the end the details didn't matter(意思是说细节，慢慢都会弄明白的) \-\-Linus Torvalds  
Linux 非常适合使用清单。  

服务器就是没有显示屏的电脑，非常酷，可以在任意电脑登录使用，可以灵活增加内存  


`pwd` 显示当前工作的绝对路径   
`ls` 参数 `-a` all `-l` list  
`cd`   

`--help` 命令更有用一些可能 `mkdir --help`  
`man` manual 显示命令的手册页，查全称查参数查用法，比如 man cat，cat 是 concatenate files and print on the standard output  
`Ctrl + r` 历史命令    



## 报错   

### [E212: 无法打开并写入文件](https://blog.csdn.net/shinyolive/article/details/108313986)     

没有权限，用户不对。比如是不是 dingyong 在改 deploy 的文件。   




