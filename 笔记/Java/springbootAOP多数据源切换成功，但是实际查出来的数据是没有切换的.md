写qalab的时候使用了AOP（面向service的）来切换多数据源，但是有个地方通过日志查看数据源是切换成功了，但是查询出来的数据不对，后来根据 [spring项目中多数据源无法切换的原因分析](https://blog.csdn.net/puhaiyang/article/details/88013712)这篇文章知道，springboot@Transactional开启事务后，会先获取你的数据源连接，然后才执行service代码，这时AOP才生效，所以数据源就不能切换了，当时的解决方法是把事务放进了 不同的service中（因为用另一个数据源的service是查询可以和后面可以不是 同一个事务）

