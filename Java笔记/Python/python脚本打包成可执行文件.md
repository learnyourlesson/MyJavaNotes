**打包成exe**

我使用了pyinstaller 直接使用

```
pip install pyinstaller 
```

安装的 有一个bug：

无法将.py转换为.exe，因为整数被接收为Byte。

![img](img/python脚本打包成可执行文件.png)

要安装最新的测试版，

```
pip install https://github.com/pyinstaller/pyinstaller/archive/develop.tar.gz
```

使用:

```
pyinstaller -F -w -pD:\tmp\core-python\libs -i d:\tmp\main.ico 【main.py】
```

​    -F 表示生成单个可执行文件；

​    -D  –onedir 创建一个目录，包含exe文件，但会依赖很多文件（默认选项）。

​    -w 表示去掉控制台窗口，这在GUI界面时非常有用。不过如果是命令行程序的话那就把这个选项删除吧！；

​    -c  –console, –nowindowed 使用控制台，无界面(默认)；

​    -p 表示你自己自定义需要加载的类路径，一般情况下用不到；

​    -i 表示可执行文件的图标。

python --version  3.8.0

pyinstaller --version 3.5

date 2019.11.21

参考链接：https://github.com/pyinstaller/pyinstaller/issues/4265

　　　　　https://blog.csdn.net/zengxiantao1994/article/details/76578421