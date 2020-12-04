# Linux配置环境变量

set  [|grep JAVA]查看当前用户所有环境变量【查看有JAVA字符的环境变量】

grep   -n   -ri   "JAVA_HOME" 查询当前文件夹下，内容中有JVAV_HOME的数据（-r 递归查询，-i 忽略大小写）

echo ${环境变量名} 也可以查看值



##### 一、在`/etc/profile`文件中添加变量 **对所有用户生效（永久的）**

 用vim在文件`/etc/profile`文件中增加变量，该变量将会对Linux下所有用户有效，并且是“永久的”。
 例如：编辑/etc/profile文件，添加CLASSPATH变量



```bash
  vim /etc/profile    
  export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```

注：修改文件后要想马上生效还要运行

```
source /etc/profile
```

不然只能在下次重进此用户时生效。

#####  二、在用户目录下的.bash_profile文件中增加变量 **【对单一用户生效（永久的）】**

 用`vim ~/.bash_profile`文件中增加变量，改变量仅会对当前用户有效，并且是“永久的”。



```bash
vim ~/.bash.profile
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```

注：修改文件后要想马上生效还要运行

```
$ source ~/.bash_profile
```

不然只能在下次重进此用户时生效。

#####  三、直接运行export命令定义变量 **【只对当前shell（BASH）有效（临时的）】**

 在shell的命令行下直接使用`export 变量名=变量值`
 定义变量，该变量只在当前的shell（BASH）或其子shell（BASH）下是有效的，shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的话还需要重新定义。

##### 四、修改.bashrc文件

在linux系统普通用户目录（cd /home/xxx）或root用户目录（cd /root）下，用指令ls -al可以看到4个隐藏文件，

.bash_history 记录之前输入的命令

.bash_logout 当你退出时执行的命令

.bash_profile 当你登入shell时执行

.bashrc 当你登入shell时执行

请注意后两个的区别：’.bash_profile’只在会话开始时被读取一次，而’.bashrc’则每次打开新的终端时，都要被读取

```
JAVA_HOME=/home/wb.guozhuru/project/java/jdk1.8.0_201
CLASSPATH=.:./bin
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```



参考链接：https://www.jianshu.com/p/ac2bc0ad3d74
