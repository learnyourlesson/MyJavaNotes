# 破解IDEA

破解教程网址：<https://www.xiaomiqiu.com/article/78>

注意的是：输入License之前要重启一下idea

出现了点击idea无反应，地址输错了，在安装目录下找到IntelliJ IDEA 2018.1.4\bin\idea.bat文件，在最后一行添加pause，用于报错后暂停，可以查看错误信息,然后点击运行，查看错误日志。
也有可能通常是破解补丁改了idea64.exe.vmoptions这个文件造成的，idea会把IDEA bin目录下的这个配置文件复制一份到C:\Users\用户名\.IntelliJIdea2019.1\config目录下再调用，只需要修改一下这个文件里面的配置文件就好了。
