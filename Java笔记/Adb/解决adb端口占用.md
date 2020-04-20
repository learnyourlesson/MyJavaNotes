# 解决adb端口占用

```
netstat -ano | findstr "5037"
```

 查找占用5037端口的pid

![image-20200323111324818](C:\Users\wb.guozhuru\AppData\Roaming\Typora\typora-user-images\image-20200323111324818.png)

```
tasklist | findstr "19932"
```

根据pid查找程序

![image-20200323111340163](C:\Users\wb.guozhuru\AppData\Roaming\Typora\typora-user-images\image-20200323111340163.png)

```
tskill PID
```

根据pid杀死程序