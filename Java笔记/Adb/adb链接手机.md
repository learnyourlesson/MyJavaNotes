# adb链接手机

```
adb version
```

查看adb版本

```
adb kill-server
```

 在关闭adb服务后，要使用如下的命令启动adb服务。

```
adb start-server
```

查看端口

```
adb devices（会有启动的作用）
```

Android 查看app包名

```
adb shell dumpsys window w |findstr \/ |findstr name= 
或者 adb shell dumpsys window | findstr mCurrentFocus
```

adb 的一些参数

```
-s 多设备是根据设备号指定设备
shell 进入手机shell
install+应用包体路径 给手机安装应用（-r 替换现有的安装）
adb pull [文件名] /源路径/
adb push [文件名] /目标路径/
adb help 查看帮助文档
```

