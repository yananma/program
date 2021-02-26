Linux 非常适合使用清单。

服务器就是没有显示屏的电脑。

Linux 非常酷，可以在任意电脑登录使用。

可以增加内存  

https://www.bookstack.cn/read/linux-command-1.6.0/command-alias.md  

#### 命令
  

使用 man 和 help 查看命令全称  

man 是 manual 的意思，比如 man cat，cat 是 concatenate files and print on the standard output  

ps: process status  

grep  过滤查找  history | grep nginx   
Global Regular Expression Print 全局正则表达式搜索  

‘|’ 管道符，表示将前面命令的处理结果传递给后面的命令处理  

df -lh 查看剩余磁盘空间，df 就是 disk file，l 是 local，h 是 human readable 的意思，就是给人看的，可以用 man df 查出 df 和 lh 的意思    

du -sh * | sort -hr 查看大小并且排序  du disk usage，s 是 summarize 只显示总内存，h 和上面一样  
查看当前文件夹大小，在文件夹内 du -sh 就行了，也可以指定目录 du -sh /var/lib/docker/  

find / -name pycharm_project_723  
find ./ -name demo.py 在当前文件夹下查找  

mv Mask_RCNN/* ./ 移动所有文件到上一层目录  

pwd   
cd  
mkdir   
rmdir  
touch  
cp文件 终点  
cp -rf pycharm_project_723/* pycharm_project_649   
rm  
rm -rf ./* 删除当前文件夹下所有文件，保留文件夹  
mv原文件名 新文件名 重命名  
mv文件 目录 移动  
cat 只读查看  
more    
\> 写入 history \> history.txt  
\>> 追加写入  
echo  
head  
tail  
ln 软链接就是快捷方式  
history  !编号 执行；常用命令  
find 范围 名称  find /home -name hello.txt；常用的查找命令，比如找 site-packages  
locate 定位文件目录  locate hello.txt  
grep 和 |  
tar -zcxf  
tar -zxvf  
alias 自己创建命令：alias myxxkt='cd /home/elearning/xxkt/'  

<br>

-r 递归

<br>

-f 强制 


apt: Advanced Packing Tool  
SSH: Secure Shell  

#### tmux 命令  

tmux new -s 名字  
tmux ls  
tmux a -t 名字  
tmux + b d  
