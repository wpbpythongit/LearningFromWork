[toc]

## 1.基础

1，_xxx表示的是protected类型的变量，不能用于 from module import *，即保护类型只允许这个类本身与子类进行访问。

2，__xxx表示的是私有类型的变量，只能允许这个类本身进行访问，连子类也不能访问。

3，__ xxx  __表示特殊变量，如 _ _  init _ _ ， _ _ del _ _ ， _ _ call _ _（魔术对象）

4.python 中的数组分为三种类型

* list列表 arr = [元素]

* Tuple元组 arr = (元素)

* Dictionary字典 arr = {k:v}

  [详解](https://www.cnblogs.com/miaoxiaonao/p/8150169.html)
  
  对数组的一些处理：
  定义空列表`pub_events_recover=[]`
  定义空字典` pub_event_recover=dict()`
  增加或修改字典中的元素`pub_event_recover['is_recover']=1`
  列表中增加字典并清空字典内容 
  
  ```python
  pub_events_recover.append(pub_event_recover) #列表中加入字典元素
  if pub_events_recover:
      pub_events.extend(pub_events_recover) #列表中扩展进另外一个列表
      del pub_events_recover[:]
  ```

5. * 进程有若干线程组成，一个进程至少有一个线程，

     ```python
     from multiprocessing import Process
     import os
     def run_proc(name):
         print(('Run child process %s (%s)...') % (name, os.getpid()))
     
     if __name__=='__main__':
         print('Parent process %s.' % os.getpid())
         p = Process(target=run_proc, args=('test',))
         print('Child process will start.')
         p.start()
         p.join()
         print('Child process end.')
     ```

   * 启动一个线程就是把一个函数传入并创建Thread实例，然后调用start()开始执行。

     ```python
     import time
     import threading
     
     # 新线程执行的代码:
     def loop():
         print('thread %s is running...' % threading.current_thread().name)
         n = 0
         while n < 5:
             n = n + 1
             print('thread %s >>> %s' % (threading.current_thread().name, n))
             time.sleep(1)
         print('thread %s ended.' % threading.current_thread().name)
     
     print('thread %s is running...' % threading.current_thread().name)
     t = threading.Thread(target=loop, name='LoopThread')
     t.start()
     t.join()
     print('thread %s ended.' % threading.current_thread().name)
     ```

     

   * 多线程和多进程的最大的不同在于，多进程中同一个变量各自有一份拷贝存在于每个进程中，互不影响，而多线程中，所有变量都由所有线程共享，所以，任何一个变量都可以被任何一个线程修改，因此线程之间共享数据最大的危险在于多个线程同时修改一个变量把内容给改乱了。

   * 多进程

     ```python
     from multiprocessing import Process
     import os
     def run_proc(name):
         print(('Run child process %s (%s)...') % (name, os.getpid()))
     
     if __name__=='__main__':
         print('Parent process %s.' % os.getpid())
         p = Process(target=run_proc, args=('test',))
         print('Child process will start.')
         p.start()
         p.join()
         print('Child process end.')
     ```

   * 进程池 启动大量子进程 批量创建子进程

     ```python
     # !/usr/bin/env python
     # -*- coding: utf-8 -*-
     from multiprocessing import Pool
     import os
     import time
     import random
     
     def long_time_task(name):
         print('Run task %s (%s)...' % (name, os.getpid()))
         start = time.time()
         time.sleep(random.random() * 3)
         end = time.time()
         print('Task %s runs %0.2f seconds.' % (name, (end - start)))
     
     if __name__=='__main__':
         print('Parent process %s.' % os.getpid())
         p = Pool(5)
         for i in range(5):
             p.apply_async(long_time_task, args=(i,))
         print('Waiting for all subprocess done...')
         p.close()
         p.join()
         print('All subprocesses done.')
     ```

   * 多线程

     ```python
     import time
     import threading
     
     # 新线程执行的代码:
     def loop():
         print('thread %s is running...' % threading.current_thread().name)
         n = 0
         while n < 5:
             n = n + 1
             print('thread %s >>> %s' % (threading.current_thread().name, n))
             time.sleep(1)
         print('thread %s ended.' % threading.current_thread().name)
     
     print('thread %s is running...' % threading.current_thread().name)
     t = threading.Thread(target=loop, name='LoopThread')
     t.start()
     t.join()
     print('thread %s ended.' % threading.current_thread().name)
     ```

   * 队列是一种数据结构，调用队列中的数据可以用不同的进程调用 

```python
# !/usr/bin/env python
# -*- coding: utf-8 -*-
from multiprocessing import Process, Queue
import os, time, random

def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get()
        print('Get %s from queue.' % value)

if __name__=='__main__':
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    pw.start()
    pr.start()
    pw.join()
    pr.terminate()
```

## 2、模块

### 1、commands

```python
cmd = "sed -n '/PRODUCT_LANGUAGE/p' /etc/protfile"
ret = commands.getoutput(cmd)
```

[详解](https://blog.csdn.net/yanyangjie/article/details/78517190)

### 2.json.load

```python
board = board_type.replace('.','_')+'.brd'
with open(drivers_path + board, 'r') as fi:
    brd = json.load(fi, 'utf-8')
```

json.load:读取json信息 [load/dump](https://www.cnblogs.com/xiaomingzaixian/p/7286793.html)

### 3.chmod 0644 0755

```python
os.chmod(file_path + filename, 0644)
os.chmod(file_path, 0755)
```

0755 -> 用户具有读/写/执行权限，组用户和其他用户具有读/写权限
0644 -> 用户具有读/写权限，组用户和其他用户具有只读权限
一般赋予目录0755权限，文件0644权限 [详解](https://blog.csdn.net/dijkstar/article/details/50645139)

 ### 4.os对文件的操作

```python
cur_path = os.getcwd() #获取当前路径
os.chdir(file_path)  #切换到某路径
#创建一个名为targz的压缩包
with tarfile.open(file_path + fi.replace('.log', '.tar.gz'), 'w:gz') as targz:
    targz.add(fi)
```

[路径](https://www.runoob.com/python/os-getcwd.html)
[tarfile模块](https://www.cnblogs.com/shona/p/11953678.html)

### 5.创建锁

[threading.Lock()](https://www.cnblogs.com/mihoutao/p/10973260.html)
避免多个线程保卫同一块数据的时候产生错误，加锁来防止这种问题。
简单来说就是加了锁之后必须被锁的线程运行完才能运行其他线程。

```python
self._status_lock = threading.Lock()
with self._status_lock:
    ret, msg = self._check(data)
    return ret, msg
```

### 6.字符串前面加u

```python
cn_map = {
    u'读命令': u'读命令-',
    u'写命令': u'写命令'
}
```

一般用在中文字符串前面，防止因为源码储存格式问题导致再次使用时出现乱码 [详解](https://blog.csdn.net/CherryChanccc/article/details/82428220?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-4-82428220.pc_agg_new_rank&utm_term=python的字典键前面有个u代表什么&spm=1000.2123.3001.4430)

### 7.split()方法

通过指定分隔符对字符串进行切片
split的用法很多很灵活可以看看下面的笔记 [详解](https://www.runoob.com/python/att-string-split.html)

```python
try:
    cmd = "sed -n '/PRODUCT_LANGUAGE/p' /etc/profile"
    ret = commands.getoutput(cmd)
    if ret is not None and len(ret) > 0:
        return 0 if ret.split("=")[1] == "zh-CN" else 1
    return 0
except Exception as ee:
    logging.getLogger().warn('get PRODUCT_LANGUAGE error: %s' % ee)
```

### 8.join()

* python中有join()和os.path.join()两个函数，
* join()用来连接字符串数组，将字符串元组列表中的元素以指定的字符(分隔符)连接生成一个新的字符串,注意是返回一个字符串
* os.path.join()将多个路径组合后返回

```python
drivers_path = os.path.join(os.environ['INSTALL_PATH'], "templates/drivers/")
```

[详解](https://www.cnblogs.com/jsplyy/p/5634640.html)

### 9.collections 容器数据类型

[website](https://docs.python.org/zh-cn/3/library/collections.html)

### 10.os.path.abspath()

获取绝对路径
[website](https://blog.csdn.net/qq_37266079/article/details/104437947)

### 11. copy deepcopy

[website](https://www.cnblogs.com/hokky/p/8476698.html)

### 12.super()

### 13.setattr() 、getattr()、hasattr()

[website](https://www.cnblogs.com/hao2018/p/11428951.html)

### 14.lower()、upper()、capitalize()、title()

都是对字符串的处理，lower()将字符串都转化为小写，upper()将字符串都转为大写，capitalize()字符串中首字母大写其余小写，title()字符串中每个单词的首字母都大写其余小写   [website](https://www.cnblogs.com/littlefive/p/10235839.html)

### 15.logging

[website](https://www.cnblogs.com/yyds/p/6901864.html)

```python
import logging
LOG_FORMAT = "%(asctime)s - %(levelname)s - %(message)s"
logging.basicConfig(filename='pracitice.INFO',level=logging.INFO,format=LOG_FORMAT)
```

## 3.web开发

理解：一个URL就代表一个html页面，在Web应用中，服务器把网页传给浏览器，实际上就是把网页的HTML代码发送给浏览器，让浏览器显示出来。而浏览器和服务器之间的传输协议是HTTP，所以：

- HTML是一种用来定义网页的文本，会HTML，就可以编写网页；
- HTTP是在网络上传输HTML的协议，用于浏览器和服务器的通信。

整个过程就是浏览器像服务器发送一个http请求，GET仅请求资源，POST会附带用户数据

1. 浏览器发送一个HTTP请求；
2. 服务器收到请求，生成一个HTML文档；
3. 服务器把HTML文档作为HTTP响应的Body发送给浏览器；
4. 浏览器收到HTTP响应，从HTTP Body取出HTML文档并显示。

WSGI 框架 [website](https://www.liaoxuefeng.com/wiki/1016959663602400/1017805733037760)
hello.py

```python
#environ:一个包含所有HTTP请求信息的dict对象
#start_response:一个发送HTTP请求响应的函数
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b'<h1>Hello, web!</h1>']
```

server.py

```python
# 从wsgiref模块导入:
from wsgiref.simple_server import make_server
# 导入我们自己编写的application函数:
from hello import application

# 创建一个服务器，IP地址为空，端口是8000，处理函数是application:
httpd = make_server('', 8000, application)
print('Serving HTTP on port 8000...')
# 开始监听HTTP请求:
httpd.serve_forever()
```



WEB框架--flask框架
app.py


```python
from flask import Flask, request, render_template
from flask import request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def home():
    return render_template('home.html')

@app.route('/signin', methods=['GET'])
def signin_form():
    return render_template('form.html')

@app.route('/signin', methods=['POST'])
def signin():
    # 需要从request对象读取表单内容：
    username = request.form['username']
    password = request.form['password']
    if username == 'wangpengbo' and password == 'password':
        return render_template('signin-ok.html', username=username)
    return render_template('form.html', message='Bad username or password', username=username)

    #if request.form['username']=='admin' and request.form['password']=='password':
        #return '<h3>Hello, admin!</h3>'
    #return '<h3>Bad username or password.</h3>'

if __name__ == '__main__':
    app.run()
```

```html
#form.html
<html>
<head>
  <title>Please Sign In</title>
</head>
<body>
  {% if message %}
  <p style="color:red">{{ message }}</p>
  {% endif %}
  <form action="/signin" method="post">
    <legend>Please sign in:</legend>
    <p><input name="username" placeholder="Username" value="{{ username }}"></p>
    <p><input name="password" placeholder="Password" type="password"></p>
    <p><button type="submit">Sign In</button></p>
  </form>
</body>
</html>

#home.html
<html>
<head>
  <title>Home</title>
</head>
<body>
  <h1 style="font-style:italic">Home</h1>
</body>
</html>

#signin-ok.html
<html>
<head>
  <title>Welcome, {{ username }}</title>
</head>
<body>
  <p>Welcome, {{ username }}!</p>
</body>
</html>
```





















