# idea远程调试

idea添加启动配置选择远程，设置ip、端口、jdk版本等参数

![添加启动配置](../\img\idea远程调试.png)



-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005

transport：监听Socket端口连接方式（也可以dt_shmem共享内存方式）；
server：=y表示当前是调试服务端，=n表示当前是调试客户端；
suspend：=n表示启动时不中断（如果启动时中断，一般用于调试启动不了的问题）；
address：=5005表示本地监听5005端口。



远程服务器启动时要在 -jar前添加 上面的参数

远程服务启动成功后，本地idea对需要调试的地方打上断点，然后项目启动开始远程调试，远程代码要与本地相同

原理简单的讲就是远程服务器把调试需要的数据通过端口发送给给本地，本地又把调试信息发给服务器

