sudo apt-get update
sudo apt install [软件包名]

克隆一个终端 ctrl+shift+T
打开一个终端 ctrl+alt+T


改变权限:
添加权限 chmod u+x [文件名] 可执行
删除权限 chmod u-x [文件名]

默认的 bash 的脚本文件 #! /bin/bash

echo fjsldjlj > [文件名]

pwd  当前工作目录路径
ls 包含的文件目录
ls -al -ll 详细文件信息
ls ~/scripts/* 目录下所有文件
ls ~/scripts/t* 目录下t开头的所有文件
ls t* 目录下t开头的所有文件

cat 显示文件内容
tail 显示文件内容

mkdir 创建文件夹
mv 重命名
rm 删除 -rf 强制删除
touch 创建文件
> 写入到文件
>>追加写入


read num  脚本中读入一个数字
-lt小于
-gt大于
-eq等于

cal 当前时间的日历
cal 10 2020 打印2020年10月的日历
cal 2020 打印2020整年的日历
bc 调用计算器
quit 退出


脚本执行 ./[文件名]


设置别名 alias cls=clear  清屏
设置别名 alias scpt='cd /home/yytong/scripts/'
删除别名 unalias cls
注:alias在重新打开终端时设置的别名会消失，可以通过shell startup scripts 解决
vim /etc/profile
ls -la *sh

| 管道指令
grep
more
|sort 排序
wc [-lwm] [文件名]统计功能
-l 统计行 -w统计词 -m统计字符


set |grep [环境变量名]
env |more 查看所有环境变量
echo $[环境变量名] 输出环境变量的值

ps -ef  可执行文件

which [指令名]查找指令所在位置
find

ls -la  当前文件目录详情

ls -la /dev/std*

&&  ||  多条指令



变量------------------------------------------------------------------------------------------
变量名=值  声明变量
$+变量名   调用变量
""   双引号能够进行变量传递
‘’   单引号不能变量传递，而是直接输出字符串
``   反引号 调用系统函数之列的 比如echo `date`    var=`date`
echo  查看变量
--------------------------------------------------
env 查看环境变量
env |less |more

set 查看所有定义的变量
set |less |more

把执行脚本文件所在的路径添加到PATH命令中(PATH=$PATH:~/scripts)，可以直接执行脚本文件(helloworld )
注：重启失效

export  将局部变量到出为全局可访问的全局变量  重启失效


环境变量的生存期：-----------将以上失效的问题变成永久化------------------
环境变量加载进程：
1.加载/etc/profile文件的内容
/etc/bash.bashrc和/etc/profile.d/*.sh
2.加载用户目录中的隐藏文件
$HOME/.bash_profile
$HOME/.bash_login
$HOME/.profile或/.bashrc


在/etc添加自己定义后的环境变量后并不能立即实现，需要重新加载使用source命令或.命令即可
/etc文件只在每次启动(开机)时加载
vim /etc/profile
source /etc/profile

修在用户目录文件时,在bash（终端）启动时生效
vim .bashrc
source ./bashrc

alias  别名 暂时修改


字符串---------------------------------------------------------------------------------

expr length “[字符串]” 查看字符串长度
charcount=`expr length “[字符串]”`  使用反引号，将字符串长度付给一个变量

expr index "[字符串]" ‘字符’  查看字符在字符串的位置，shell索引从1开始而不是0

expr substr "[字符串]" [起始位置][长度] 字符串截断，返回子字符串

expr match "[字符串]" ’[正则表达式]‘   字符串匹配
expr "[字符串]"[空格]:[空格]‘[正则表达式]’

数字操作----------------------------------------------------------------------
两种表达方式：
expr expression 空格间隔，运算符需要加\转义

result=$[expression] 表达式中 = 要用 ==,result后面的=要紧挨着result
echo result

浮点运算--------------------------------------------------------------------------
bc 内建计算器
scale=n  设置小数点位数
quit 退出bc
bc -q不显示欢迎条 
同行声明变量用;区分
脚本中用bc  利用|管道进行
输入重定向var=`bc<<EOF   expressions  EOF`
函数-----------------------------------------------------------------------------
$? 函数调用退出码，函数成功调用默认返回0
$# 参数个数 if[$# -eq 2]判断输入是不是两个
$@ 传递进来的所有参数，数组, 定义数组array=(1 2 3 4 5)
echo $[array[*]]输出数组内容
newarray=(`echo "#@"`)把一个数组内付给另一个新的数组
local 定义局部变量,否则默认全局变量


函数库-----------------------------------------------------------------------

source或.  [文件名]  引入函数库
写入.bashrc文件

输入输出-----------------------------------------------------------
1.输入输出重定向
输入从键盘重定向为文件输入
输出从显示器重定向为文件输出

输出：
command > 文件路径  echo 2222 > ./tf4    echo 2222 > tf4
command >> 文件路径
输入：
command < 文件路径  cat < tf4
同时指定输入输出 cat < tf4 > tf5

内联输入重定向：
command << marker
data input
marker

2.文件描述符与错误重定向
linux系统将每个对象当作文件处理，包括输入输出过程，用文件描述符标识文件对象，
文件描述符时非负整数，唯一标识会话中打开的文件，每个过程一次最多可以有9个文件描述符
文件描述符 缩写  描述
0	STDIN  标准输入
1	STDOUT 标准输出
2	STDERR 标准错误

输入输出重定向：（实际上输入输出重定向不需要写出0，1）
command 1> 文件路径
command 0> 文件路径
错误信息重定向：（必须写出 2），用于将错误写入日志中，跟踪错误
command 2> 文件路径

用法：分离正误输出位置 1>文件路径 2>文件路径

3.脚本中重定向输入输出
临时重定向：
定义 command >$文件描述符
执行 ./脚本 2>> errlog
或 command >>errlog

永久重定向:
输入：
定义 exec 0<errlog
输出：
定义 exec 1>文件
错误：
定义 exec 2>文件
定义 echo "fsdds" >$2

4.管道  |

将一个命令的输出，重定向至另一个命令的输入
command | command

ps -ef  查看运行中的进程
ps -ef | less | more
ps -ef |grep bash

ps -ef >tf6
less < tf6

tee 命令：一个输入两个输出,既能在屏幕输出，又能输出到文件
date | tee datefile


条件判断------------------------------------------------------------

1.if-then
##################
if command   执行成功，退出码=0，表示command成功，执行then
then
	commands
fi
##################
if command;then
	commands
else
	commands
fi
##################
if command1;then
	commands
elif command2;then
	commands
else
	commands
fi
#################
test命令：条件成立时，返回退出码0，否则返回1；
if test condition
then
	commands
if
或使用[]
if [condition];then
	commands
if

2.常用条件判断方法

复合判断[condition] && [condition]||[condition]
可以实现三类条件：数值比较，字符串比较，文件比较
read num  脚本中读入一个数字
数值比较：
n1 -lt n2小于
n1 -gt n2大于
n1 -eq n2等于
n1 -ge n2大于等于
n1 -le n2小于等于
n1 -ne n2不等于

字符串比较:
str1 = str2 相同
str1 != str2 不同
str1 \> str2 str1比str2大,逐个比较字符串assic码值的大小
str1 \< str2 str1比str2小,
-n str1	字符串长度是否非0
-z str1 字符串长度是否为0

文件比较：
-d file 检查file是否存在并且是一个目录
-e file 检查file是否存在
-f file 检查file是否存在并且时一个文件
-s file 检查file是否存在并且非空
file1 -nt file2 检查file1是否比file2新
file1 -ot file2 检查file1是否比file2旧

-r file 检查file是否存在并且可读
-w file 检查file是否存在并且可写
-x file 检查file是否存在并且可执行
-O file 检查file是否存在并且属于当前用户所有
-G file 检查file是否存在并且默认组与当前用户相同

高级判断：
((表达式))高级数字表达式
if (($val ** 2 > 90))
((val2=$val ** 2))

[[表达式]]高级字符串比较：模式匹配
if [[$USER == y*]] 调用环境变量


3.case

语法：从上到下匹配条件，字符串或数字都可以
case variable in 
pattern1 | pattern2) commands1;;
pattern3) 
commands3;;
*) 
default commands;;
esac

循环------------------------------------------------------------------
for
while
until

break
continue

for循环：
#################
for var in list
do 
	commands
done
################
for((i=1;i<=10;i++))
do
	commands
done

list来源：直接列表，从变量读取，从命令读取，从目录读取
列表默认分隔符：空格，换行符，制表符
修改分隔符：IFS=$'\n' 只保留换行

从目录读取：
for var in ~/scripts/* 所有文件
for var in ~/scripts/t* t开头的文件


while/until
######################
while:当test成立时，进入循环
while test command
do
	commands
done
######################
until:当test成立时，退出循环
until test command
do
	commands
done


循环和嵌套
###################
计数器：
i=1
i=$[ $i +1 ]

用户输入处理-----------------------------------------------------------------

用户输入分类：
1命令行参数（选项，参数）
2运行时输入

命令行参数：------------
1读取参数：
参数之间空格分隔
位置参数：$+position : $0,$1,$2,...$9
$0:程序名
$1,$2....$9
参数超过9个，${10}....

2读取程序名：
basename

echo `basename $0` 返回真实脚本名，但不包括路径

ls -s 软连接

3特殊变量：
$# 参数计数
$* 所有参数，所有参数当作一个参数存储
$@ 参数列表，当作一个列表


命令行参数处理：-----------------
1错误检测：
条件判断：
if [$# -lt 2]....exit

2shift命令：多参数处理,参数向前移动，一次输入一个数，遍历所有参数
循环中while.......shift 

3 选项处理
简单选项
case...esac shift
分离参数和选项:
-- 特殊字符隔离，结束选项判断，开始处理参数,shift break;;
带值选项：
getopt命令：帮助处理带值选项的值
getopts命令：帮助处理带值选项的值
while getopts ab:c opt.....$opt
shift $[ $OPTIND -1 ]

选项的标准化：------------------
-a 显示所有对象
-c 生成一个计数
-d 指定一个目录
-e 扩展一个对象
-f 指定读入数据的文件
-h 显示命令的帮助信息
-i 忽略文本大小写
-l 产生输出的长格式版本
-n 使用非交互模式
-o 指定将所有输出重定向的输出文件
-p 以安静模式运行
-r 递归处理目录和文件
-s 以安静模式运行
-v 生成详细输出
-x 排除某个对象
-y 对所有问题回答yes



在脚本中获取用户输入-------------------------
1 基本读取
read [变量名] 
2 超时处理:-t  单位：秒
read -t 5 -p "-p的输入提示文本"：输入时间不大于5秒时，进行正常读入
3 隐藏方式读取：比如输入密码
read -s passwd:隐藏输入，付给变量passwd
4 文件中读取：文件重定向或管道
#####################
exec 0< 文件
while read line  从文件读数据，付给line变量
do
	...
done
###########
cat 文件 | while read line
do
	...
done


脚本运行控制-------------------------------------------------------
1 linux中的信号

man命令 查看支持的信号

产生信号：-------------
键盘组合 
终止进程 SIGINT CTRL+C
暂停进程 SIGSTP CTRL+Z

常用命令：终止进程
kill
killall

kill -9 无条件终止进程

处理信号----------------
程序中的信号处理：
	按照默认方式处理信号
	忽略信号
	按照自定义方式捕捉并处理信号 使用trap
		trap "fsdsfsd" SIGINT 在输入ctrl+c时捕捉并输出文本
		trap "fsdsfsd" EXIT 在退出时捕捉并输出文本			
		trap - EXIT 移除，此时不再输出之前定义的文本

2 后台运行脚本
不运行在终端显示器的脚本

命令格式
SCRIPT $  即脚本名字后面加一个 $
jobs 查看所有作业
fg [作业号] 从前台形式重新启动停止的作业/脚本
bg [作业号] 从后台形式重新启动停止的作业/脚本

作业优先级：-20（高） -- 19（低）
nice -n 10 ./脚本 > temp &  指定优先级为10执行后台执行脚本，输出重定向到临时文件
ps al 查看优先级情况 

renice 10 -p 19863  修改运行中的优先级为10
普通用户只能指定优先级0--19
sudo用户/root用户可以指定更高优先级 sudo renice 10 -p 19863 

以上，关闭终端时，后台脚本会停止
解决方法：后台脚本与bash控制台分离
命令格式
nohup SCRIPT $
nohup ./脚本 &  此时，关闭控制台，后台仍在运行，产生一个nohup.out文件

3 定时运行脚本
命令格式:
at [-f filename ] time
时间格式:
10：15
10：15～PM
now noon midnight teatime
MMDDYY MM/DD/YY MM.DD.YY
Jul 14 Dec 25
now+25min

系统不自带at命令，需要安装 sudo apt-get install at
at命令默认将标准输出和标准错误发送到邮箱中，日常使用中，需要重定向
at -M   不用邮箱

at -M -f ./脚本 time

atq 查看at命令正在队列中的脚本
atrm 作业号  删除队列中的脚本

指定周期运行-----
cron 命令
查看命令格式：vim /etc/crontab
ls /etc/cron*

用户cron表的编辑:
crontab -l 查看
crontab -e 添加一个时间表，修改当前用户的计划时间表

异步  anacron anacrontab ,不能处理频率大于一天一次的脚本

4 启动时运行脚本
系统启动时运行------------

开机过程：system V int     upstart int

自定义开机运行脚本：
debian /etc/init.d/rc.local
ubuntu /etc/rc.local
opensuse /etc/init.d/boot.local
centos /etc/rc.d/rc.local

Tomcat 服务器的知识

shell启动时运行-----------
1 启动bash   运行/etc/profile  /etc/.bashrc
2 通过ssh登陆 运行/etc/profile  /etc/.bashrc
3 通过ssh执行命令 运行/etc/.bashrc    不会经过login shell过程，命令需要写入.bashrc中,需要加入条件判断











