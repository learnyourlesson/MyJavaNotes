# uwsgi配置


```
[uwsgi]

# 外部访问地址，可以指定多种协议，现在用http便于调试，之后用socket,socket外部不能访问

http = 10.246.44.185:15500

# 指向项目目录
chdir =  ...


# 虚拟运行环境目录。 一般情况下，在进行完整的 python 项目开发时，都会创建一个虚拟环境，用于安装依赖的包。如果需要运行的项目有虚拟环境的话，可以在这里指定其目录。
virtualenv = /home/raoweijian/project/python/login_transfer/venv

# flask启动程序文件
wsgi-file = ...
# flask在manage.py文件中的app名
callable = app

# 处理器数
processes = 4

# 线程数
threads = 2

buffer-size = 8192

#状态检测地址
stats = 127.0.0.1:5051

# master :允许主线程存在（true）
master=True

# uwsgi 的进程号存放文件
pidfile=uwsgi.pid

# 日志输出
daemonize=uwsgi.log
```

 我这边有个问题，正式服和测试服在一个服务器上，环境变量只能同时配置一个，我是export [name]=[value]来临时先配置了环境变量在重启uwsgi