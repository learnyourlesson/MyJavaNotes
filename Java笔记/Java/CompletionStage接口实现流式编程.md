**CompletionStage接口实现流式编程：**

JDK8新增接口，此接口包含38个方法...是的，你没看错，就是38个方法。这些方法主要是为了支持函数式编程中流式处理。

 

**4.3.CompletableFuture中4个异步执行任务静态方法：**

![img](../img/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B1.png)

如上图，其中supplyAsync用于有返回值的任务，runAsync则用于没有返回值的任务。Executor参数可以手动指定线程池，否则默认ForkJoinPool.commonPool()系统级公共线程池，注意：这些线程都是Daemon线程，主线程结束Daemon线程不结束，只有JVM关闭时，生命周期终止。

**4.4.组合CompletableFuture：**

**thenCombine()：** **先完成当前CompletionStage和other 2个CompletionStage任务，然后把结果传参给BiFunction进行结果合并操作。**

 

![img](../img/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B2.png)

**thenCompose()：第一个CompletableFuture执行完毕后，传递给下一个CompletionStage作为入参进行操作。**

![img](../img/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B3.png)

**demo:**

JDK CompletableFuture 自带多任务组合方法allOf和anyOf

**allOf**是等待所有任务完成，构造后CompletableFuture完成

**anyOf**是只要有一个任务完成，构造后CompletableFuture就完成

![img](../img/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B/CompletionStage%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0%E6%B5%81%E5%BC%8F%E7%BC%96%E7%A8%8B4.png)

方式一：循环创建CompletableFuture list,调用sequence()组装返回一个有返回值的CompletableFuture，返回结果get()获取

方式二：全流式处理转换成CompletableFuture[]+**allOf**组装成一个无返回值CompletableFuture，join等待执行完毕。返回结果whenComplete获取。---》推荐

 

```java
1 package thread;
  2 
  3 import java.util.ArrayList;
  4 import java.util.Date;
  5 import java.util.List;
  6 import java.util.concurrent.CompletableFuture;
  7 import java.util.concurrent.ExecutorService;
  8 import java.util.concurrent.Executors;
  9 import java.util.stream.Collectors;
 10 import java.util.stream.Stream;
 11 
 12 import com.google.common.collect.Lists;
 13 
 14 /**
 15  * 
 16  * @ClassName:CompletableFutureDemo
 17  * @Description:多线程并发任务,取结果归集
 18  * @author diandian.zhang
 19  * @date 2017年6月14日下午12:44:01
 20  */
 21 public class CompletableFutureDemo {
 22     public static void main(String[] args) {
 23         Long start = System.currentTimeMillis();
 24         //结果集
 25         List<String> list = new ArrayList<String>();
 26         List<String> list2 = new ArrayList<String>();
 27         //定长10线程池
 28         ExecutorService exs = Executors.newFixedThreadPool(10);
 29         List<CompletableFuture<String>> futureList = new ArrayList<>();
 30         final List<Integer> taskList = Lists.newArrayList(2,1,3,4,5,6,7,8,9,10);
 31         try {
 32             ////方式一：循环创建CompletableFuture list,调用sequence()组装返回一个有返回值的CompletableFuture，返回结果get()获取
 33             //for(int i=0;i<taskList.size();i++){
 34             //    final int j=i;
 35             //    //异步执行
 36             //    CompletableFuture<String> future = CompletableFuture.supplyAsync(()->calc(taskList.get(j)), exs)
 37             //        //Integer转换字符串    thenAccept只接受不返回不影响结果
 38             //        .thenApply(e->Integer.toString(e))
 39             //        //如需获取任务完成先后顺序，此处代码即可
 40             //        .whenComplete((v, e) -> {
 41             //            System.out.println("任务"+v+"完成!result="+v+"，异常 e="+e+","+new Date());
 42             //            list2.add(v);
 43             //        })
 44             //        ;
 45             //    futureList.add(future);
 46             //}
 47             ////流式获取结果：此处是根据任务添加顺序获取的结果
 48             //list = sequence(futureList).get();
 49             
 50             //方式二：全流式处理转换成CompletableFuture[]+组装成一个无返回值CompletableFuture，join等待执行完毕。返回结果whenComplete获取
 51             CompletableFuture[] cfs = taskList.stream().map(object-> CompletableFuture.supplyAsync(()->calc(object), exs)
 52                     .thenApply(h->Integer.toString(h))
 53                     //如需获取任务完成先后顺序，此处代码即可
 54                     .whenComplete((v, e) -> {
 55                         System.out.println("任务"+v+"完成!result="+v+"，异常 e="+e+","+new Date());
 56                         list2.add(v);
 57                     })).toArray(CompletableFuture[]::new);
 58             //等待总任务完成，但是封装后无返回值，必须自己whenComplete()获取
 59             CompletableFuture.allOf(cfs).join();
 60             System.out.println("任务完成先后顺序，结果list2="+list2+"；任务提交顺序，结果list="+list+",耗时="+(System.currentTimeMillis()-start));
 61         } catch (Exception e) {
 62             e.printStackTrace();
 63         }finally {
 64             exs.shutdown();
 65         }
 66     }
 67     
 68     public static Integer calc(Integer i){
 69         try {
 70             if(i==1){
 71                 //任务1耗时3秒
 72                 Thread.sleep(3000);
 73             }else if(i==5){
 74                 //任务5耗时5秒
 75                 Thread.sleep(5000);
 76             }else{
 77                 //其它任务耗时1秒
 78                 Thread.sleep(1000);
 79             }
 80             System.out.println("task线程："+Thread.currentThread().getName()+"任务i="+i+",完成！+"+new Date());  
 81         } catch (InterruptedException e) {
 82             e.printStackTrace();
 83         }
 84         return i;
 85     }
 86     
 87     /**
 88      * 
 89      * @Description 组合多个CompletableFuture为一个CompletableFuture,所有子任务全部完成，组合后的任务才会完成。带返回值，可直接get.
 90      * @param futures List
 91      * @return
 92      * @author diandian.zhang
 93      * @date 2017年6月19日下午3:01:09
 94      * @since JDK1.8
 95      */
 96     public static <T> CompletableFuture<List<T>> sequence(List<CompletableFuture<T>> futures) {
 97         //1.构造一个空CompletableFuture，子任务数为入参任务list size
 98         CompletableFuture<Void> allDoneFuture = CompletableFuture.allOf(futures.toArray(new CompletableFuture[futures.size()]));
 99         //2.流式（总任务完成后，每个子任务join取结果，后转换为list）
100         return allDoneFuture.thenApply(v -> futures.stream().map(CompletableFuture::join).collect(Collectors.toList()));
101     }
102    
103     /**
104      * 
105      * @Description Stream流式类型futures转换成一个CompletableFuture,所有子任务全部完成，组合后的任务才会完成。带返回值，可直接get.
106      * @param futures Stream
107      * @return
108      * @author diandian.zhang
109      * @date 2017年6月19日下午6:23:40
110      * @since JDK1.8
111      */
112     public static <T> CompletableFuture<List<T>> sequence(Stream<CompletableFuture<T>> futures) {
113         List<CompletableFuture<T>> futureList = futures.filter(f -> f != null).collect(Collectors.toList());
114         return sequence(futureList);
115     }
116 }
```

 

方式二返回结果：

```
task线程：pool-1-thread-1任务i=2,完成！+Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-9任务i=9,完成！+Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-6任务i=6,完成！+Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-8任务i=8,完成！+Mon Jun 19 18:26:17 CST 2017
任务6完成!result=6，异常 e=null,Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-4任务i=4,完成！+Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-7任务i=7,完成！+Mon Jun 19 18:26:17 CST 2017
任务4完成!result=4，异常 e=null,Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-3任务i=3,完成！+Mon Jun 19 18:26:17 CST 2017
任务3完成!result=3，异常 e=null,Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-10任务i=10,完成！+Mon Jun 19 18:26:17 CST 2017
任务10完成!result=10，异常 e=null,Mon Jun 19 18:26:17 CST 2017
任务7完成!result=7，异常 e=null,Mon Jun 19 18:26:17 CST 2017
任务8完成!result=8，异常 e=null,Mon Jun 19 18:26:17 CST 2017
任务2完成!result=2，异常 e=null,Mon Jun 19 18:26:17 CST 2017
任务9完成!result=9，异常 e=null,Mon Jun 19 18:26:17 CST 2017
task线程：pool-1-thread-2任务i=1,完成！+Mon Jun 19 18:26:19 CST 2017---》任务1耗时3秒
任务1完成!result=1，异常 e=null,Mon Jun 19 18:26:19 CST 2017
task线程：pool-1-thread-5任务i=5,完成！+Mon Jun 19 18:26:21 CST 2017---》任务5耗时5秒
任务5完成!result=5，异常 e=null,Mon Jun 19 18:26:21 CST 2017
list2=[6, 4, 3, 10, 7, 8, 2, 9, 1, 5]list=[],耗时=5076---》符合逻辑，10个任务，10个线程并发执行，其中任务1耗时3秒，任务5耗时5秒，耗时取最大值。
```

**建议:CompletableFuture满足并发执行，顺序完成先手顺序获取的目标。而且支持每个任务的异常返回，配合流式编程，用起来速度飞起。JDK源生支持，API丰富，推荐使用。**
**建议:CompletableFuture满足并发执行，顺序完成先手顺序获取的目标。而且支持每个任务的异常返回，配合流式编程，用起来速度飞起。JDK源生支持，API丰富，推荐使用。**

**5.总结：**

本文从原理、demo、建议三个方向分析了常用多线程并发，取结果归集的几种实现方案，希望对大家有所启发，整理表格如下：

|                        | **Futrue**                              | **FutureTask**                                               | **CompletionService**                                        | **CompletableFuture**                               |
| ---------------------- | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------- |
| **原理**               | Futrue接口                              | 接口RunnableFuture的唯一实现类，RunnableFuture接口继承自Future<V>+Runnable: | 内部通过阻塞队列+FutureTask接口                              | JDK8实现了Future<T>, CompletionStage<T>2个接口      |
| **多任务并发执行**     | 支持                                    | 支持                                                         | 支持                                                         | 支持                                                |
| **获取任务结果的顺序** | 支持任务完成先后顺序                    | 未知                                                         | 支持任务完成的先后顺序                                       | 支持任务完成的先后顺序                              |
| **异常捕捉**           | 自己捕捉                                | 自己捕捉                                                     | 自己捕捉                                                     | 源生API支持，返回每个任务的异常                     |
| **建议**               | CPU高速轮询，耗资源，可以使用，但不推荐 | 功能不对口，并发任务这一块多套一层，不推荐使用。             | 推荐使用，没有JDK8**CompletableFuture**之前最好的方案，没有质疑 | **API极端丰富，配合流式编程，速度飞起，推荐使用！** |

```java
CompletableFuture[] results = channels.stream().map(object-> CompletableFuture.runAsync(() -> {
           ...
        }, threadPoolTaskExecutor)).toArray(CompletableFuture[]::new);
CompletableFuture.allOf(results).join();
```



原文链接：https://www.cnblogs.com/dennyzhangdd/p/7010972.html#_label2