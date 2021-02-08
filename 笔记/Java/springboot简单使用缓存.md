# springboot使用缓存

**基于注解的缓存**

修改Application类，加入启用缓存的注解@EnableCaching

```java
@SpringBootApplication
@EnableCaching
public class CacheSimpleApplication {
  public static void main(String[] args) {
    SpringApplication.run(CacheSimpleApplication.class, args);
  }
}
```

@EnableCache注解启动了Spring的缓存机制，它会使应用检测所有缓存相关的注解并开始工作，同时还会创建一个CacheManager的bean,可以被我们的应用注入使用。

**几个注解**

- @EnableCaching 启用缓存配置
- @Cacheable 指定某个方法的返回值是可以缓存的。在注解属性中指定缓存规则。
- @Cacheput 将方法的返回值缓存到指定的key中
- @CacheEvict 删除指定的缓存数据

**注意**

@Cacheable和@Cacheput都会将方法的执行结果按指定的key放到缓存中，@Cacheable在执行时，会先检测缓存中是否有数据存在，如果有，直接从缓存中读取。如果没有，执行方法，将返回值放入缓存，而@Cacheput会先执行方法，然后再将执行结果写入缓存。 **使用@Cacheput的方法一定会执行**

#### **参数详解**

##### **@Cacheable可以指定三个属性，value、key和condition**

 key属性是用来指定Spring缓存方法的返回结果时对应的key的。该属性支持SpringEL表达式。当我们没有指定该属性时，Spring将使用默认策略生成key



自定义策略是指我们可以通过Spring的EL表达式来指定我们的key。这里的EL表达式可以使用方法参数及它们对应的属性。使用方法参数时我们可以直接使用“#参数名”或者“#p参数index”。下面是几个使用参数作为key的示例。

  

```java
@Cacheable(value="users", key="#id")
public User find(Integer id) {
    returnnull;
} 

@Cacheable(value="users", key="#p0")
public User find(Integer id) {
    returnnull;
} 

@Cacheable(value="users", key="#user.id")
public User find(User user) {
    returnnull;
}


@Cacheable(value="users", key="#p0.id")
public User find(User user) {
    returnnull;
}
```

 

​    除了上述使用方法参数作为key之外，Spring还为我们提供了一个root对象可以用来生成key。通过该root对象我们可以获取到以下信息。

| 属性名称    | 描述                        | 示例                 |
| ----------- | --------------------------- | -------------------- |
| methodName  | 当前方法名                  | #root.methodName     |
| method      | 当前方法                    | #root.method.name    |
| target      | 当前被调用的对象            | #root.target         |
| targetClass | 当前被调用的对象的class     | #root.targetClass    |
| args        | 当前方法参数组成的数组      | #root.args[0]        |
| caches      | 当前被调用的方法使用的Cache | #root.caches[0].name |

 

​    当我们要使用root对象的属性作为key时我们也可以将“#root”省略，因为Spring默认使用的就是root对象的属性。如：

```java
@Cacheable(value={"users", "xxx"}, key="caches[1].name")
public User find(User user) {
    returnnull;
}
```

######  condition属性指定发生的条件

​       有的时候我们可能并不希望缓存一个方法所有的返回结果。通过condition属性可以实现这一功能。condition属性默认为空，表示将缓存所有的调用情形。其值是通过SpringEL表达式来指定的，当为true时表示进行缓存处理；当为false时表示不进行缓存处理，即每次调用该方法时该方法都会执行一次。如下示例表示只有当user的id为偶数时才会进行缓存。

```
@Cacheable(value={"users"}, key="#user.id", condition="#user.id%2==0")
public User find(User user) {
	System.out.println("find user by user " + user);
	return user;
}
```

##### @CachePut 和 @Cacheable参数一样

##### @CacheEvict

​    @CacheEvict是用来标注在需要清除缓存元素的方法或类上的。当标记在一个类上时表示其中所有的方法的执行都会触发缓存的清除操作。@CacheEvict可以指定的属性有value、key、condition、allEntries和beforeInvocation。其中value、key和condition的语义与@Cacheable对应的属性类似。即value表示清除操作是发生在哪些Cache上的（对应Cache的名称）；key表示需要清除的是哪个key，如未指定则会使用默认策略生成的key；condition表示清除操作发生的条件。下面我们来介绍一下新出现的两个属性allEntries和beforeInvocation。

###### allEntries属性

​    allEntries是boolean类型，表示是否需要清除缓存中的所有元素。默认为false，表示不需要。当指定了allEntries为true时，Spring Cache将忽略指定的key。有的时候我们需要Cache一下清除所有的元素，这比一个一个清除元素更有效率。

```java
@CacheEvict(value="users", allEntries=true)
public void delete(Integer id) {
    System.*out*.println("delete user by id: " + id);
}
```

 

###### beforeInvocation属性

​    清除操作默认是在对应方法成功执行之后触发的，即方法如果因为抛出异常而未能成功返回时也不会触发清除操作。使用beforeInvocation可以改变触发清除操作的时间，当我们指定该属性值为true时，Spring会在调用该方法之前清除缓存中的指定元素。

  

```java
@CacheEvict(value="users", beforeInvocation=true)
public void delete(Integer id) {
	System.*out*.println("delete user by id: " + id);
}
```

## @Caching

​    @Caching注解可以让我们在一个方法或者类上同时指定多个Spring Cache相关的注解。其拥有三个属性：cacheable、put和evict，分别用于指定@Cacheable、@CachePut和@CacheEvict。

 

```java
@Caching(cacheable = @Cacheable("users"), 
         evict = { @CacheEvict("cache2"),@CacheEvict(value = "cache3", allEntries = true) })
public User find(Integer id) {
	return null;
}
```



参考链接：https://blog.csdn.net/lchq1995/article/details/80265105

https://www.cnblogs.com/fashflying/p/6908028.html