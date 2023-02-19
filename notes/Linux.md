## 基本命令
passwd 新密码   更改用户名

passwd 密码     (给root改)

userdel -r 用户 (删除用户以及文件)

useradd  用户名

su root (切换用户)

groupadd 组名 (加组)

usermod -g 更改的组 更改的用户

useradd -g 更改的组 创建用户

init 0关机 3无图形 5有图形

cp 源文件地址  去的地址

cp -r 源文件夹地址  去的地址

/cp -r 源文件 去的地址  强制覆盖

mkdir -r 

mkdir p 创建文件目录 ---mkdir -p 11/{11,22,33}

mv 源文件或地址   去的文件或地址    (文件名一样就移动，不一样就修改并移动)

cat more/less  (可以减少空间)

echo 输出内容到控制台

head -n 5 文件 前五行

tail -n 5 文件 后五行  

tail -f 实时跟踪 -F

* > 覆盖写入      >>追加写入

cal 日历

ln -s 要链接的文件或目录 要创建的链接名  ( 软链接)

！编号   (执行历史记录)

ls-l -lh -a

find 地址 （-name -user -saze）名字，用户，大小

sync 保存同步

locate 文件名 

which ls  查询指令位置

grep -n "ls" /home/shaohuan/笔记 (筛选查找)

gzip 文件地址文件名 

qunzip 文件地址文件名

zip -r压缩目录

unzip -d 解压到哪个目录

tar -c打包-v信息-f压缩的文件名-z打包同时压缩 tar -zcvf 压缩后文件名  压缩前文件

tar -zxvf 解压  解压文件（目录） -C 解压到文件目录

  

## 组

* 所有者：

chown 更改后所有者 要更改的文件（修改文件的所有者）

chown 更改后所有者：更改后组 要更改文件    

chown -R 更改后所有者 更改前文件夹（递归文件夹）

* 组：

groupadd  

chgrp 组名 文件名 （修改文件/目录的组）

* 其他组：

usermod -g 新组名 用户名 useradd -g 新组名 用户名 usermod -d 初始目录 用户名

* 修改权限

chmod g o a        

chmod g=rwx/o=rwx/a=rwx  文件名 

chmod g/o/a+-rwx421

  

## 任务调度

  

crond(每次都调用)

1.写脚本调用, (记得赋权限)2.直接写入指令调用

crontab -e编辑 -l查询 -r删除终止所有任务service crond restart 重启 

* * * * *  分 时 天 月 星期    */1 * * * *  代表每隔一分钟执行  

at(只调用一次)

atd (执行进程队列) ps - ef | grep atd 现在的进程运行

at (选项) 04:00/04am/04:00 /2021-3-1   /now + 5 days   

ctrl D 两次 输出完成

atq 未执行进程 

atrm 5 删除编号5 未执行进程

  

## 磁盘分区

  

### lsblk -f (查看分区)

1. 虚拟机加载硬盘

2. 重启系统

3. 分区命令

fdisk /dev/sdb

n 新分区  p主分区  w 写入退出

4. 格式化分区

mkfs -t ext4 /dev/sdb1

5. 挂载

mount /dev/sdb1 挂载位置

unmount

vim /etc/fstab 添加 磁盘分区 mount -a 自动生效

  

### df -h 磁盘分区细节

du -s汇总 -a文件 -h细节 --max-depth=1查询深度 -c明细加汇总     文件/文件夹占用情况

ls -l /home | grep "^-" | wc -l      查询home文件夹中文件的个数

tree 文件夹  

  

  

## 网络

  

更改网络静态ip  vi /etc/sysconfig/network-scripts/ifcfg-ens33    

更改主机名  vim /etc/hostname

更改主机host映射   vim /etc/hosts  

  
## 进程

ps  -a -u -x

ps -aux 或者-ef  所有进程细节

pid 进程id  ppid 父进程id   -ef 可以查看父进程

killall 进程名称（通配符） 杀死相关所有进程 

kill 进程号 

kill -9 进程号 强制杀死 

pstree -u 进程所属用户  -p 显示进程的PID

## 服务

systemctl start | stop | restart | status  

systemctl 查看指令管理的服务 /usr/lib/systemd/system

systrmctl enable disable 服务名   自启/关闭自启

systemctl is-enabled 服务名 查看是否自启

systemctl  stop firewalld  (临时停止)

firewall-cmd--permament--add-port=端口号\协议(111/tcp) 添加端口号到防火墙

firewall-cmd--permanent--remove-port=端口号/协议 移出防火墙

firewall-cmd--reload 重新加载生效

firewall-cmd--query-port=端口号/协议 查询

systemctl get-default 查看当前状态（1-5） 

systemctl set-default 名字

## 动态监控进程

 top 和ps很像 但是他可以更新监控

-d 秒数  -i 不显示僵尸进程 -p 只监控某个进程

P cpu排序 M 内存排序 N pid排序  q 退出

u  输入监控用户 

k 输入进程id号 结束进程

  

## 监控网络状态进程

netstat -an 一定排列输出  -p 名字 查询那个进程在调用 

  

  

## rpm包管理

rpm -qa 查询所安装软件包

    -qa | more 分页

    -qa | grep 过滤

    -q name 查询软件是否安装

    -qi name 查询信息

    -ql name 查询软件包中文件

    -qf 文件路径 查询文件归属于那个软件包

rpm -e name 删除文件包

    --nodeps 强制卸载

rpm -ivh 文件包全路径 安装提示进读条 

  

  

yum 软件包管理 应用商店

install / list  

  

## shell编程

.sh

./name  本文件夹内执行文件 或者绝对路径

1，编辑脚本要  
```
#！/bin/bash开头
```

  
2.要有执行权限

或者sh name不用执行权限也能执行 

或者source name

export 变量    当前进程变量传递到子进程

  

系统变量 

$HOME PWD SHELL USER PATH系统变量 系统设置好的 set 查看所有系统变量

  

自定义变量

变量名=值  --不能加空格 --名字大写

A=`值` 反引号运行其中命令   不然只赋值 --A=$(值)等价

unset 撤销变量

readonly变量  不可修改 不可撤销

  

环境变量

vim /etc/profile

export变量名=变量值   --添加环境变量

source配置文件目录   --立即生效

echo $变量名    --查询 

  

多行注释

：<<!

内容

！

  

位置参数（传参）

./执行命令名 第一个参数  第二个参数。。。

$n 0代表命令本身 $1-$9代表1-9的参数 十个以上${10}

$* 所有参数一起对待

$@ 所有参数分别对待

$# 参数个数

  

预定义变量

$$ 当前pid   $! 最后一个进程pid  $? 最后一个命令的成败0为真 

  

运算符

  

1、$((运算式子))  2、$[运算式子]  3、expr + - \* / 加减乘除（带空格）

条件判断

if [ "ok"="ok" ]  带空格记得

then

    echo="equal"

elif[判断] 

   echo ""

fi

  

字符串比较

=

整数比较

-lt 小于  -le 小于等于 -eq 等于 -gt 大于 -ne不等于 

  

  

case 语句

  

case $变量名 in

"值1")

code

如果变量等于值1 就执行1

;;

"值2")

如果变量等于值2 执行2

code

;;

*)

code

如果都不是执行这个

;;

esac

  

  

for语句

  

第一种

for i in "$*"   --表示i的循环取值为$*,或者$@

do

   echo "num is $i"

done

  

第二种

for (( 初始值;循环条件;变量变化 ))   

do

code

done

  

  

例题：1加到100

SUM=0

for(( i=1; i<=100; 1++))

do

  SUM=$[$SUM+$i]

done

echo "SUM=$SUM"

  

while循环

  

while [ 条件判断式 ]

do

code

done 

  

SUM=0

i=1

while [ $i -le $1 ]

do

     SUM=$[$SUM+$i]

     i=$[$i+1]

done

   echo "$SUM"

  

  

  

read获取输入

read -p "提示信息" 参数名  -t 指定读取时间 "提示信息" 参数名

read -p "请输入=" NUM1 --赋值给NUM1

echo "NUM1=$NUM1" 

  

系统函数

basename 完整路径/文件名.txt 

输出：文件名.txt

  

dirname 完整路径/文件名 

输出：完整路径

  

自定义函数

function getSum() {

     SUM=$[$n1+$n2]

    echo “sum=$SUM”

}

