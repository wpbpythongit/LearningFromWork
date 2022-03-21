



# linux

[toc]



## 1、netstat

```shell
netstat -ap | grep ' program'
```

[详解][https://blog.csdn.net/bit_clearoff/article/details/60876695]

-------------------

## 2、删除

```shell
rm -rf 
```

---------------------

## 3、解压

```shell
tar -xzvf xxx.tgz -C xxx  //解压到xxx文件
```

[详解][https://www.linuxcool.com/tar]

----------------

## 4、改名

```shell
mv 原文件名 新文件名
```

--------------------

## 5、远程拷贝目录

```shell
scp -r /opt/data/mariabackup root@192.168.29.171:/opt/data  
//将本机mariabackup文件拷贝到192.168.29.171的/opt/data路径下
```

----------------------

## 6、更改文件的用户和群组

```shell
chown -R mysql:mysql /opt/data/mysql 
//将/opt/data/mysql文件用户和群组都改为mysql
```

[详解][https://www.linuxcool.com/chown]      [详解2][https://www.cnblogs.com/djane/articles/4867371.html]

-----------------------

## 7、查看进程

``` shell 
ps  -ef | grep +进程名
```

[详解][https://www.linuxcool.com/ps]

-------------------------------

## 8、查看日志

``` shell
tail -f /opt/log/xstorage.log
cat /var/log/messages   //查看系统日志
cat /var/log/messages | grep Keepalived  //查看双机切换的日志
tail -f /opt/data/xrocket/log/xrocket.log  //安装部署的日志
tail -f /opt/log/xstorage.log      //单模块xstorage的日志
tail -f /opt/log/xbroker.V2.log    //单模块xbroker.V2的日志
```

[详解][https://www.linuxcool.com/tail]

```shell
grep --colour=auto 'keyword' /opt/log/board_agent*
tail -F board_agent* | sed -n '/keyword/p'
tail -F board_agent* | sed -n '/keyword/w log1'
```




---------------

## 9、查看系统版本

```shell
uname -a 
cat /proc/version  //显示linux内核版本号
cat /etc/redhat-release   //显示centos版本号
```

----------------

## 10.tcpdump抓包

```shell
ifconfig   #查看网卡号
netstat -tnp | grep foreginport#查看端口号
tcpdump -i enp2s0 host 192.168.5.244 and port 161 -T snmp -vvv
tcpdump -i enp6s0f0 host 61.32.0.105 and port 161 -w a.cap
```

[ifconfig命令输出详解](https://blog.csdn.net/qq_37050329/article/details/89887579)

## 11.vim异常关闭

比如vim打开1_1_2.py文件时，异常关闭，就会产生一个.1_1_2.py.swp的交互文件，可用`vim -r 1_1_2.py` 来恢复文件，  然后可用`ls -al` 命令或者`ls -al | grep 1_1_2`显示所有文件,然后进行删除，这样就能正常打开1_1_2.py文件了。如果不想产生交换文件可用命令`set noswapfile`,同样可用`set swapfile`设置生成交换文件，这两个设置仅对当前文件生效。默认设置交换文件会隔4000毫秒或者200个字符保存一次，可通过命令修改 

```shell
set updatetime=8000
set updatecount=800
```

## 12.反编译

安装uncompyle2，解压tar -xf uncompyle2-1.1.tar.gz，cd  uncompyle2-1.1，python setup.py install；反编译命令：uncompyle2 X.pyc > X.py;

## 13. find

```shell
find / -name FILENAME -print
```

## 14.ls命令显示文件时间大小

```shell
ls -lht --time-style=long-iso
```



[https://www.linuxcool.com/tar]: 