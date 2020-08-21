# 使用supervisor自动部署服务

supervisor 是一个py的工具可以帮助我们自动部署服务，主要是写一个配置文件放在其服务端的配置文件中配置的位置上 supervisord.conf 

![](..\img\服务端指定的要管理的进程的配置文件放置处.png)

然后编写要管理的进程的配置文件

```
[program:scheduling-tool-dev]
; 工作目录
directory=/home/wb.guozhuru/scheduling-tool/schedulingtool/target
; java用jar方式启动进程，并配置参数
command=/home/wb.guozhuru/scheduling-tool/java/jdk1.8.0_201/bin/java -jar schedulingtool-1.0-SNAPSHOT.jar --spring.profiles.active=dev
; 如果是true,当supervisor启动时,程序将会自动启动
autostart=true
; 日志最大20MB
stdout_logfile_maxbytes = 20MB
; 日志保留数
stdout_logfile_backups = 20
; 日志放置的位置
stdout_logfile = /home/wb.guozhuru/scheduling-tool/logs/logs-dev.log
; 自动重启
autorestart=true
; 这个是启动进程需要的环境变量，怕影响其他东西就没配置，使用了supervisor来管理，多个用逗号隔开
environment=JAVA_HOME="/home/wb.guozhuru/scheduling-tool/java/jdk1.8.0_201/bin"
```

在项目中编写一个linux脚本

```
# 更新项目
git pull
# maven打包项目
bash /home/wb.guozhuru/scheduling-tool/maven/apache-maven-3.6.3/bin/mvn clean package
# 启动进程
sudo supervisorctl restart scheduling-tool-dev
```

注意：1.第一次的时候supervisorctl update更新supervisor服务，让它加载你新加的配置文件

​			2.sudo 命令可能要权限和密码，不知道可以修改**sudo为不需要密码**，加权限：编辑/etc/sudoers文件，到一行root ALL=(ALL)  ALL的下一行，your_user_name ALL=(ALL)  ALL

这样就把自己加入了sudo组，可以使用sudo命令了。（这样是要密码的，不太清楚密码怎么设置的）默认5分钟后刚才输入的sodo密码过期，下次sudo需要重新输入密码，如果觉得在sudo的时候输入密码麻烦，把刚才的输入换成如下内容即可：your_user_name ALL=(ALL) NOPASSWD: ALL

​			3. git pull的时候如果使用的http每次都要输入用户密码，可以换成ssh的方式或者git config --global credential.helper store，修改git的配置然后git pull/git push 输入一次账号密码，就会把你的账号密码记录下来了（store是明文），如何关闭不太清楚 ，保存的地方是可以指定的，默认不清楚

参考链接：[supervisor](https://juejin.im/entry/5c1f01616fb9a049cd543042)

​					[sudo设置权限和无密码](https://www.cnblogs.com/itech/archive/2009/08/07/1541017.html)

​					[git设置辅助程序保存密码](https://www.cnblogs.com/volnet/p/git-credentials.html)