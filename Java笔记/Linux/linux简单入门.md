netstat -tln 查看端口占用

netstat -pant | grep 8080 查看8080端口占用

cat /etc/issue 查看linux发行版

ps -ef | grep mongo 查看有关mongo的进程

ps -aux | grep mongo  查看有关mongo的进程

kill PID 杀死PID对应的进程

nohup java -jar app.jar >logs.txt 2>&1 &（最后一个&命令放到后台执行，nohub是进程后台执行   2>$1 错误日志（2是错误日志，1是正常日志）（&1重定向到正常日志中）也重定向到logs.txt中）

vim操作

i 编辑模式

esc 命令模式

先命令模式，，然后

:w 保存文件但不退出vi

:w file 将修改另外保存到file中，不退出vi

:w! 强制保存，不推出vi

:wq 保存文件并退出vi

:wq! 强制保存文件，并退出vi

q: 不保存文件，退出vi

:q! 不保存文件，强制退出vi

:e! 放弃所有修改，从上次保存文件开始再编辑


参考链接：http://caibaojian.com/vim.html