# 可视化导入导出mongoDB数据

一开始使用的是 mongoDB compass

![mongoDB compass](..\img\可视化导入导出mongoDB数据1.png)



但是导入有问题，原表中有数据就导入不了

后来通过robo3t 的

![robo3T](..\img\可视化导入导出mongoDB数据2.png)

的插入直接复制json进去然后验证一下（两个对象之间的json最多只能有一个\n），就可以保存了

