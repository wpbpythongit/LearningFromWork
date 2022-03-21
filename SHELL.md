# SHELL

-----

[toc]

-----

## 1.$0 $1 $2 $# $?

**$0**表示脚本名称
**$1--$9**表示脚本执行时传入的参数1到参数9
**$#**脚本执行时输入的参数个数
**$?**脚本的返回值

-----

## 2.systemctl

Linux 服务管理两种方式service和systemctl
systemd是Linux系统最新的初始化系统(init),作用是提高系统的启动速度，尽可能启动较少的进程，尽可能更多进程并发启动。
systemd对应的进程管理命令是systemctl

`systemctl [command] [unit]`
`command`: start 	stop	restart	reload	enable	disable ......  [常用命令详解][https://blog.csdn.net/skh2015java/article/details/94012643]

-----

## 3.source

source命令也被称为“点命令”，也就是一个点符号 . ,是bash的内部命令。
功能：使shell读入指定的shell程序文件并依次执行文件中的所有语句。相当于`sh test.sh` 
但是和`./test.sh`不一样，此处的点表示当前目录。[具体区别][https://blog.csdn.net/bible_reader/article/details/81776896]

------

## 4.${var}    $(cmd)

`$var`是取得变量的值，用`${var}`可以更精确的界定变量名称的范围，也就是说`${var}`后面还可以接东西，比如

```shell
 sync_host="root@${OPPOSITE_ROLE}.xbrother.com"
```

  他还可以取路径、文件名、后缀，[详见][https://www.cnblogs.com/chengd/p/7803664.html]

$(cmd)用来替换命令

-----

## 5.echo -e 

激活转义字符。双引号是解析出变量输出，单引号是原封不动的输出。
[具体用法][https://www.cnblogs.com/karl-python/p/9261920.html]

----

![image-20210927102154544](C:\Users\xb\AppData\Roaming\Typora\typora-user-images\image-20210927102154544.png)

---------------

##  6.sed

` sed -option 'command/.../'`
利用sed筛选日志：
`tail -F board_agent* | sed -n '/keyword/p'`
`tail -F board_agent* | sed -n '/keyword/w log1'`  

### 1.s替换命令

```shell
#命令行替换
$ echo "This is a test" |  sed 's/test/big test/'
This is a big test
#替换文件内容
$ cat data1
The quick brown fox jumps over the lazy dog.
The quick brown fox jumps over the lazy dog.
$ sed 's/dog/cat/' data1
The quick brown fox jumps over the lazy cat.
The quick brown fox jumps over the lazy cat.
#替换标记 s/pattern/replacement/flags
#<数字>替换第几处,将每行中第二个dog替换成cat
sed 's/dog/cat/2' data 
#<g>替换所有,将每行中所有dog替换成cat
sed 's/dog/cat/g' data
#<p>输出发生替换的行，和-n选项配合,-n会禁止sed自动输出，p选项会输出匹配行
sed -n 's/dog/cat/p' data
#<w>和p功能相同，但是会输出到指定的文件里面
sed -n 's/dog/cat/w data2' data1
```

### 2.删除命令

删除命令必须配合寻址模式使用，否则流中所有行都会被删除

```shell
sed '3d' data  #删除第三行
sed '2,3d' data  #删除二到三行
```



### 3.-e选项  执行多个命令

```shell
$ sed -e 's/brown/green/; s/dog/cat/' data1
The quick green fox jumps over the lazy cat.
The quick green fox jumps over the lazy cat.
```

### 4.-f选项  从文件中读取编辑器命令

```shell
$ cat script1
s/brown/green/
s/fox/elephant/
s/dog/cat/
$ sed -f script1 data1
The quick green elephant jumps over the lazy cat.
The quick green elephant jumps over the lazy cat.
```

### 5.行寻址 指定行数处理

```shell
sed '2s/dog/cat/' data  #只将文件第二行进行替换处理
sed '2,3s/dog/cat/' data  #只处理文件的第二到三行
sed '2,$s/dog/cat/' data  #处理文件的第二行到最后一行
sed '2{s/fox/elephant/; s/dog/cat/}' data  #对文件第二行进行多条命令处理
```





## 7.gawk程序

`gawk -option '{command}' /.../`

gawk程序可以提供一个类编程环境，提供了一种编程语言，他是把文件一行一行处理的，会自动给每行中的每个数据元素分配一个变量,`$0` 代表整个文本行，`$1`代表文本行中第一个数据字段，以此类推，gawk中默认的字段分隔符是任意的空白字符，如想改变字段分隔符需要用`-F` 选项

```shell
$ gawk -F: '{print $1}' /etc/passwd  
#以冒号作为字段分隔符来打印passwd文件每行中第一个数据字段
```

```shell
$ echo "My name is Rich" | gawk '{$4="Christine"; print $0}'
#执行多条命令用分号隔开
```

### 1.BEGIN END关键字

​     `BEGIN`关键字强制gawk在读取数据前执行BEGIN关键字后指定的程序脚本；`END`关键字会使gawk在读完数据后执行END指定的程序脚本。

```sh
$ cat script
BEGIN {
print "The latest list of users and shell"
print "Userid     Shell"
print "-------    -------"
FS=":"
}

{
print $1 "               " $7
}

END{
print "This concludes the listing"
}
```

-------------------

## 0.实践 解决现场bug脚本

```shell
#!/bin/bash
#This script helps you deal with problems more easily

function log {
	clear
	tail -F /opt/log/board_agent*
}

function drivers {
	clear
	echo -n "Enter the ID:"
	read ID
	ls /opt/xbrother/xacquisition/templates/drivers | grep $ID
}

function groups_brd {
	clear
	vim /opt/xbrother/xacquisition/templates/groups_brd.json
}

function xbconstruct {
	clear
	vim /opt/xbrother/xacquisition/templates/xbconstruct/xbconstruct.py
}

function path {
	echo -e "log:/opt/log
		    \ndrivers:opt/xbrother/xacquisition/templates/drivers
		    \n......"
}

function help {
 	clear
 	echo "help"
}

function menu {
	clear
	echo
	echo -e "\t\t\tDealer Menu\n"
	echo -e "\t1.path"
	echo -e "\t2.log"
	echo -e "\t3.drivers"
	echo -e "\t4.groups_brd.json"
	echo -e "\t5.xbconstruct.py"
	echo -e "\t6.help"
	echo -e "\t0.Exit program\n\n"
	echo -en "\t\tEnter option:"
	read -n 1 option
}

while [1]
do 
	menu
	case $option in 
	0)
		break ;;
	1)
		path ;;
	2)
		log ;;
	3)
		drivers ;;
	4)
		groups_brd ;;
	5)
		xbconstruct ;;
	6)
		help ;;
	*)
		echo "Sorry,wrong selection" ;;
	esac
	echo -en "\n\n\t\tHit any key to continue"
	read -n 1 line
done
clear
```













































