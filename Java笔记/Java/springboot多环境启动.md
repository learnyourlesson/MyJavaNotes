**1. Environment配置**

Configuration ---> Environment ---> Program arguments 中输入指令：--spring.profiles.active=dev

**2. Environment配置（配置环境参数）**

Configuration ---> Environment ---> Environment variables 中输入参数：spring.profiles.active=dev

**3. Active profiles配置**

Configuration ---> Active profiles 中输入dev （指定启动文件）

Override parameters可以指定启动时所需的参数

**Maven 方式启动**

![img](img/springboot%E5%A4%9A%E7%8E%AF%E5%A2%83%E5%90%AF%E5%8A%A8/springboot%E5%A4%9A%E7%8E%AF%E5%A2%83%E5%90%AF%E5%8A%A81.png)

![img](img/springboot%E5%A4%9A%E7%8E%AF%E5%A2%83%E5%90%AF%E5%8A%A8/springboot%E5%A4%9A%E7%8E%AF%E5%A2%83%E5%90%AF%E5%8A%A82.png)

Parameters配置如上图所示

**1. Runner配置**

Runner ---> Environment variables 中配置：spring.profiles.active=dev

**2.Runner配置**

Runner ---> Skip tests 中配置：spring.profiles.active=dev

原文链接：https://juejin.im/post/5d5bacccf265da03e5233690