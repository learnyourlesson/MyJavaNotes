# Json反序列化内部类报错

内部类会有一个外部类的引用，在反序列化时，无法获取这个这个引用，会报无法创建这个对象的错误，在内部类上加一个static 就可以了



参考链接：https://blog.csdn.net/MrHamster/article/details/85269119