

可以组合使用 tail 和 head 切片日志。       

多用 man    
f forward    
b backward     


shell 脚本是程序员工具箱的重要组成部分，既适用于个人，也适用于大型任务。如果你发现自己一遍又一遍地运行着同样的命令序列，那就把它们放到 shell 脚本中，从而将烦琐工作变得自动化。   

哪天按照书的目录再重新排列一下笔记里内容的顺序       


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

`find` 一般要用通配符 `find ./ -name community*`      
`find ./ -name *feed-*` 不是 feed 开头，前面要加通配符     
`find` 范围 名称  `find /home -name hello.txt`  `-name` `-size` `-user`  
`find ./ -name manage.py`   
`find ./ -name *.html`   
`locate` 定位文件目录 `locate hello.txt`  
find命令及不显示Permission denied：`find [path] -name "pattern " 2>/dev/null`，比如 `find ./ -name upload_to_wei* 2>/dev/null`    

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

`ls` 使用通配符，要用 grep。`ls | grep .*2022-12-12.*`    
`ls` 文件名中包好某个字 `ls | grep .*日`（注意是 .\*日，不能只写 \*日），比如 `ls | grep 通用.*01-30`    
`ls -lht` t 是时间，按时间排序，用来删不用的日志，删除日志代码：`print(*[line.split()[-1] for line in log_str.splitlines()])`      
`ls -lhrt` r 是 reverse 倒序     


`mkdir` 一次创建多级目录，使用参数 \-p parents `mkdir -p /home/animal/dog`    
`cp` `cp file dir`   
复制文件夹和文件夹下所有文件：`cp -r source_dir(最后不加斜杠) dest_dir(最后加斜杠)`  
在当前路径备份：`cp -r eval_ribao ./eval_ribao_bak/`    
复制多个文件 `cp a.txt b.txt dir/`  


scp -P 对方端口、自己文件或压缩包、对方用户名@对方内网 IP、目标文件夹。  
`scp -P 17717 a.txt b.txt c.txt deploy@192.168.241.3:/home/deploy/`        
`scp -P 17717 -r little_env deploy@192.168.241.25:/home/deploy/` 文件夹 -r    
带密码 scp，可以用于定时任务。
`sshpass -p "密码，外面要带上前后这两个引号" scp -P 17717 /home/test/syb/mayanan/msg_encryptor/crontab_ps.txt deploy@192.168.241.30:/opt/cyberin_backend/tmp/crontab_ps.txt`        
比如：`sshpass -p 'ylQUpQ&s4bwNiqZr' scp -P 17717 679da677ec4ec8ce72379070a9c1c5f.png deploy@192.168.241.26:/usr/local/nginx/html/static/tx_daily/img/upload_img`     

带密码跳转，比如：`sshpass -p 'ylQUpQ&s4bwNiqZr' ssh -p 17717 b26`        
速度非常快，100MB/s  

`yum install lrzsz`    
z 是 ZMODEM 协议。    
rz 上传文件到服务器 receive，用鼠标拖拽。   
sz 下载文件到本地 send，可以使用通配符：`sz *、sz *.jpg`、`sz *.xls*`    

`mv` `mv a.txt dir1`    
`mv` 原文件名 新文件名 (目的地在当前目录，就是在这个目录下操作就不是移动了，就是重命名)     
`mv week_data/ tools/` 本来 week_data 和 tools 是同级目录，mv 完以后就变成了 tools/week_data/ 形式     
`mv /data/new /data/old/` 剪切文件夹和文件夹下内容  
`ls -lhtr | tail -n 3 | awk '{print $9}' | xargs -i mv {} ../b/`      

rm 可以删除多个文件：rm a.txt b.txt     
`rm` `rm -r 目录名` 删除非空目录，r recursive  
`rm -rf video_frame/` 删除文件夹下所有文件，删除文件夹  
`rm -rf video_frame/*` 删除当前文件夹下所有文件，保留文件夹  
`rm *2023-01-30_17*` 通配符     
`ls | grep export_tongyong_excel_ | xargs -i rm {}` 删除 ls 的结果    
`rm` 反向删除：开启 extglob 选项 `shopt -s extglob`; 删除 `rm -rf !(文件或文件夹，多个用 | 分隔)`; 关闭 extglob 选项 `shopt -u extglob`   
`python -c "import shutil; shutil.rmtree('目录')"` 递归删除文件夹       

`rmdir` 删除空目录  
`ln -s 原文件 快捷方式`，创建软连接的时候，快捷方式文件不能存在，如果存在要先删掉再创建  
`ln -s train_trainv6_norm.json train.json`  
ls 显示的时候，前面的是快捷方式，后面的是原文件  

`which` 显示可执行文件的位置，`which python`、`which ipython`   
`help` 获取 Shell 内建命令的帮助信息    

`alias` 创建自己的命令：`alias myxxkt='cd /home/elearning/xxkt/'`  
可以使用参数，比如 `alias c744="chmod 744 $1"`; 比如 `alias lmn="less -m -N $1"`  
alias 可以和多条命令结合，命令之间用 ; 分隔   
或者用一条命令，比如：`alias ipython='cd /opt/cyberin_backend/ && /home/deploy/cyberin_env/bin/python manage.py shell --settings cyberin_backend.settings_product'`    
alias 永久生效：`$ vim ~/.bashrc`、`source ~/.bashrc`   
堡垒机如果删除了 bashrc 里的 alias ，source 以后不生效，就重连堡垒机。     


`cat` 只读查看  
cat 可以和 grep 结合使用，看代码有没有更新：`cat /opt/cyberin_backend/accounts/models.py | grep show_warning`    
`sort`  
`grep` Global Regular Expression Print 全局正则表达式搜索 `history | grep nginx` `ps -aux | grep nginx`    
grep 正则过滤多个：`pip list | grep -E "a|b"`    
grep 展示附近多行：-A NUM, --after-context=NUM；-B NUM, --before-context=NUM；-C NUM, -NUM, --context=NUM    
fgrep 不支持正则，纯文本匹配。   

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
可能看不了别的用户，比如 deploy 可能就看不到 dingyong 的进程，需要用 root 看      

如果启动服务说端口被占用，lsof 结果为空，很可能是还处在 4 次挥手阶段，等一会儿再试。     

`ps` process status   
`ps -aux | less`  
`ps -aux | grep Nginx`   

`ps -aux` 可以查看所有用户的进程，不单单是当前用户的进程。    

`ps -eO lstart | grep zkpoint_es` 查看进程开始时间   
`ps -aux --sort=%mem` 按内存占用排序      
`ps -aux --sort=%mem | head -n 50` 取 top50     
`ps -aux --sort=-%mem | head -n 20 > mayanan/crontab_ps/$(date +%Y_%m_%d_%H:%M:%S)_ps.txt` 拼接日期文件名      
`echo $(date +"%Y %m %d %H:%M:%S") "     " $load_average`       


如果想看定时任务的脚本的参数，一个是可以看 crontab，如果是 root 用户启用的 crontab，没有权限查看，可以先看日志，看命令执行时间，然后在定时任务执行的时候执行 `ps -aux | grep post_sim` 看正在执行的命令。        


`ps -ef | less`  
可以查看父进程  


Ctrl + z 挂起进程    
jobs 看有哪些被挂起的进程    
fg 然后回车进入进程，看文章说可以加进程参数，但是试了不生效不知道为什么      





kill 进程 id  
\-9 强制关闭  

可以同时 kill 多个进程   

kill \-9 id1, id2, id3...

`ps -aux | grep 名字 | awk -F " " '{print $2}'| xargs kill -9`    
`ps -aux | grep -v grep | grep zishengtang | awk '{print $2}' | xargs kill -9`         


pstree  


`top` 是动态更新的  

top P 按 CPU 排序，是默认显示样式  
top M 按内存排序  
top T 按运行时间排序    
top N 按 pid 排序   
通过 xb 显示是按照哪一列排序。    

top c 显示命令全称  

o过滤，写 COMMAND=python；= 取消过滤。   
m可以切换内存占用进度显示方式。   

TIME/TIME+ CPU 运行时间。   

-E 切换剩余内存占用显示单位。   
-e 切换下面进程内存占用显示单位。   

top 命令是有配置文件的，也就是说你通过命令修改的配置都可以保存下来。保存配置的命令为大写字母 W。在你修改了 top 命令的配置后按下大写字母 W，然后退出 top 命令并再次执行 top 命令，此时你的修改仍然在起作用。    

-i 不显示空闲进程。   




htop   

安装：`conda install -c conda-forge htop`，其中 conda-forge 是一个 channel，相当于应用商店      


查看 CPU 核数：lscpu 看 CPU(s)         

uptime 查看 load average。    
可以结合 uptime 和 awk 实现监测功能。     
`uptime | awk -F "load average: " '{print $2}' | awk -F "," '{print $1, $2, $3}' `    


`watch -d -n 0.5 nvidia-smi`   
d 展示变化的地方   
n 时间间隔，默认是 2s    



#### 网络 
`ifconfig`  
内网 ip：`ifconfig | grep 192`    
外网 ip：`ifconfig | grep 112`    

当命令里有端口的时候，就可以通过 ps 命令找到进程，比如 Python 启动的带端口的 django 服务。   

当命令里没有端口的时候，就要通过 netstat 找到命令。   

```shell
netstat -anp | grep 15200

结果：tcp        0      0 192.168.241.35:15200    0.0.0.0:*               LISTEN      7685/python

然后再用 ps 找到命令
ps -aux | grep 7685

```

`netstat -anp | less`  




`curl` 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端(client)的 URL 工具的意思。   

`curl http://localhost:18775/accounts/get_verify_img/`     
`curl http://127.0.0.1:18775/accounts/get_verify_img/`     
`curl http://192.168.241.79:18775/accounts/get_verify_img/`     

带 session 请求：`curl -b "cyberin_session=c3vvamldlbj8m9o" "http://127.0.0.1:18795/xposts/voice_general/?objectid=3026&start_date=10-20&end_date=10-21"`     

POST application/x-www-form-urlencoded     
`curl -d "param1=value1&param2=value2" -X POST http:///localhost:3000/data`     

[curl post 各种情况](https://blog.csdn.net/aa390481978/article/details/115082240)    

`wget` 复制要下载的文件链接，用 `wget 复制的 url` 下载   
`axel` 比 wget 更强大的多线程下载工具，以后用到的时候再研究一下       


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


# Shell 编程  

运行脚本，两种方式   

1. sh a.sh   
2. ./a.sh     


### 语法  

```shell
# for 循环
命令行使用 for i in a b c d;do echo $i; done

```


shell 比传统的编程语言要简单很多，如果学过其他的学这个就是小菜一碟(找例子多敲就会了)  


### 删除文件

vim rm_txt.sh   

```shell  
#!/bin/bash
rm num.txt
``` 

chmod 755 rm_txt.sh   

定时任务     
0 0 * * * cd /home/deploy/msg_encryptor/data && sh rm_txt.sh   


### 更新定时任务   

update_crontab.sh     

```shell
#!/bin/bash 
cd /opt/cyberin_backend && crontab -l > crontab_b79.txt   
git pull
git add crontab_b79.txt   
git commit -m "update crontab.txt" 
git push   
```    


### 过滤删除   

```shell
#!/bin/bash 
cd tongyong_report/ && ls | grep "预发" | xargs -I % unlink %
echo "rm pre send done."
```


### 截断日志   

```shell 
#!/bin/bash
cd /opt/cyberin_backend/logs

# 更新互动数日志
tail -n 1000000 tongyong_update_interact_num.log > trunc_tongyong_update_interact_num.log
mv trunc_tongyong_update_interact_num.log tongyong_update_interact_num.log
echo "tongyong_update_interact_num.log done."
```


### 进程监测脚本  

send_top_ps.sh
```shell
load_average=$(uptime | awk -F'load average:' '{print $2}' | awk -F',' '{print $1}')
echo $load_average
if (( $(echo "$load_average > 40" | bc -l) )); then
    ps_output=$(ps aux --sort=-%cpu | head -n 21)
    python ~/mayanan/send_feishu_msg.py --msg "$ps_output"
fi
``` 


### 删除多个文件   

delete_latest_file.sh  
```shell
#!/bin/bash

if [[ ! -z $1 ]]; then
    dir="$1"
else
    dir="./"
fi

if [[ ! -z $2 ]]; then
    cnt="$2"
else
    cnt=1
fi

latest_files=$(ls -lhtr "$dir" | tail -n "$cnt" | awk '{print $9}')
for latest_file in $latest_files; do
    echo "delete $dir$latest_file"
    'r'm "$dir$latest_file"
done
```



# 实际例子  

### 删除文件名中包含“日”的文件   

```shell
ls | grep .*日 | xargs -i rm {}    
```

```shell
ls | grep "预发" | xargs -I % unlink %
``` 


### 按时间排序，删除 ls -lht 结果中的前 5 条   

```shell 
# 先查看结果：     
ls -lht | head -n 6 | tail -n 5 | awk '{print $9}'

# rm 删除   
ls -lht | head -n 6 | tail -n 5 | awk '{print $9}' | xargs rm -f

# unlink 删除   
ls -lht | head -n 6 | tail -n 5 | awk '{print $9}' | xargs -I % unlink %
```


### 包含 buzz 不包含 pre_send     

```shell    
crontab -l | grep buzz | grep -v pre_send 
```


# 日志    

### 日志显示切片    

```python 
tail crisis_warning_send_log.log.log -n 700 | head -n 100     
```


### 在所有日志里过滤    


过滤上数据日志（最常用的方法）     

b16 dingyong 用户     

```shell
grep -r --include='(文件名)tidb-*' '（要查的字符串）https://12345.haikou.gov.cn/' （文件路径）/home/dingyong/logs/uploader/         
```


```shell
ansible 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81 -m shell -a "grep -r --include='tidb-*' 'https://haikou.cn/' /home/dingyong/logs/uploader/"         
```



```shell 
grep -r 'Traceback' .(. 是起始路径)    
```


在所有 .py 文件里查找    

通配符、查询内容、路径      

```shell 
grep -r --include='*.py' '192.168.241.51' .(. 是起始路径)    
```


在指定文件名的日志里过滤     

```shell
find . -name "*iesdouyin_com*" | xargs grep "7233248167658016061"      
```

指定文件名在多台服务器上过滤内容      

```shell 
ansible 6,11,45 -m shell -a "find log/ -name \"*iesdouyin_com*\" | xargs grep \"7233248167658016061\" | grep \"2023-05-16 02\""
``` 



```shell   
cat * | grep http://www.douyin.com/video/123343546576876978
```

在 scripts 文件夹的所有脚本里找命令行参数       

```shell   
cat * | grep run_gmtopic.py     
``` 

正则    

```shell  
cat * | grep -E "run_gmtopic.py(.*?)config"   
```  


### 日志文件太大，导出部分日志  

```shell 
tail -n 30000 crisis_admin.log > trunc_crisis_admin.log    
``` 

### 日志文件太大，只保留最后几十万条日志   

```shell 
tail -n 300000 crisis_admin.log > trunc_crisis_admin.log  
mv trunc_crisis_admin.log > crisis_admin.log    
``` 


### 统计日志 SQL 查询大于 5 秒的查询  

不用正则：    
```shell 
tail -n 200 origin_20220611124948.log | grep 正在处理第65组聚类结果：
```


正则：    
```shell    
cat debug.log | grep -E "\((\d{2,}|[5-9])\.\d+\)[^\r\n]+" > slow_sql.log   
```

可以直接用正则，没写 -E 也可以，有默认的正则类型    
```shell
tail -n 500 push_oulaiya.log | grep [^08:]06:.*done.    
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

chmod +x ./* 给当前文件夹下的所有文件添加执行权限      



#### crond 任务调度  

定时执行特定的命令或程序  

[网页工具 https://tool.lu/crontab/](https://tool.lu/crontab/)(还有不少其他工具，可能有用)   

分钟、小时、日期、月份、周几      

\* 取值范围内的所有数字    
/ 每过多少个数字   
\- 从X到Z   
, 散列数字    

crontab 备份：crontab -l > crontab_b33_20230925.txt    

**使用 crontab 前，先执行 crontab -l，因为输入 crontab -e 编辑时，很可能会输成 crontab -r 清空了定时任务。先输 l，清空了可以复制**   
\-l list  
\-e edit  
\-r remove  
`crontab -l` 列表  
`crontab -e` 编辑   

crontab 命令，时间前面加 0 和不加 0 都是可以的，默认不加 0   
0 8 * * *     
0 08 * * * 
效果是一样的。       


`*/20 * * * *  # 每 20 分钟执行一次`     
`0 */2 * * *` # 每 2 个小时执行一次，**注意一定要指定分钟是 0，如果不指定，就会每分钟都会执行一次**    
`0 8 * * * cd /home/test/syb/mayanan/zjgdk && /home/test/anaconda3/envs/zjgdk/bin/python main.py &>> command/logs/upload_to_zjgdk_daily_task.log`    
`0 9,13,16 * * * cd /home/deploy/crisis_admin && /home/deploy/.crisis/bin/python manage.py push_zishengtang --settings=crisis_admin.settings_new_product &> /home/deploy/crisis_admin/logs/zishengtang.log &`    
`0 12-22/2 20-24 10 *` 10 月 20-22 号，12-22 点，每两个小时执行一次。         
``

[菜鸟教程例子](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)      


#### apt 软件管理  
apt-get update  
apt-get install package  
apt-get remove package  

apt-cache search package  
apt-cache show package  
apt-get install package --reinstall  

所谓源，就是手机里面的应用商店  


#### 查看平台的操作系统类型和版本，是 centos 还是 Ubuntu

centos  
cat /etc/centos-release    


Ubuntu    
lsb_release -a    


#### tmux 命令  

tmux new -s 名字  
tmux ls  
tmux a -t 名字  
tmux + b d  


docker 改时区；`mv /etc/localtime /etc/localtime.bak && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`      

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




