**mongoDb 原始连接方式准备工作**

首先我们需要驱动，MongoDB的Java驱动我们可以直接在Maven中央仓库去下载，也可以创建Maven工程添加如下依赖：

```html
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver</artifactId>
    <version>3.5.0</version>
</dependency>
```

建议通过Maven来添加依赖，如果自己下载jar，需要下载如下三个jar：**（驱动版本要一致）**

1. org.mongodb:bson:jar:3.0.1				https://oss.sonatype.org/content/repositories/releases/org/mongodb/bson/3.0.1/

2. org.mongodb:mongodb-driver-core:jar:3.0.1	https://oss.sonatype.org/content/repositories/releases/org/mongodb/mongodb-driver-core/3.0.1/

3. org.mongodb:mongodb-driver:jar:3.0.1		https://oss.sonatype.org/content/repositories/releases/org/mongodb/mongodb-driver/3.0.1/

另外，在使用Java操作MongoDB之前，记得启动MongoDB哦~

访问 http://127.0.0.1:27017/

![img](img/mongoDb 原始连接方式1.png)

**获取集合**

所有准备工作完成之后，我们首先需要一个MongoClient，如下：

```java
MongoClient client = new MongoClient("192.168.248.136", 27017);
```

然后通过如下方式获取一个数据库，如果要获取的数据库本身就存在，直接获取到，不存在MongoDB会自动创建：

```java
MongoDatabase sang = client.getDatabase("sang");
```

然后通过如下方式获取一个名为c1的集合，这个集合存在的话就直接获取到，不存在的话MongoDB会自动创建出来，如下：

```java
MongoCollection<Document> c = sang.getCollection("c1");
```

有了集合之后，我们就可以向集合中插入数据了。

**增**

和在shell中的操作一样，我们可以一条一条的添加数据，也可以批量添加，添加单条数据操作如下：

**想要mongoDB支持localDate等要使用jackson-datatype-jsr310转换localDate类型，JDBC（关系型数据库连接规范）不支持 Instant**

```java
Document d1 = new Document();
d1.append("name", "三国演义").append("author", "罗贯中");
c.insertOne(d1);
```

添加多条数据的操作如下：

```java
List<Document> collections = new ArrayList<Document>();
Document d1 = new Document();
d1.append("name", "三国演义").append("author", "罗贯中");
collections.add(d1);
Document d2 = new Document();
d2.append("name", "红楼梦").append("author", "曹雪芹");
collections.add(d2);
c.insertMany(collections);
```

**改**

可以修改查到的第一条数据，操作如下：

```java
c.updateOne(Filters.eq("author", "罗贯中"), new Document("$set", new Document("name", "三国演义123")));
```

上例中小伙伴们也看到了修改器要如何使用，不管是inc，用法都一致，我这里不再一个一个演示。也可以修改查到的所有数据，如下：

```java
c.updateMany(Filters.eq("author", "罗贯中"), new Document("$set", new Document("name", "三国演义456")));
```

**删**

可以删除查到的一条数据，如下：

```java
c.deleteOne(Filters.eq("author", "罗贯中"));
```

也可以删除查到的所有数据：
```java
c.deleteMany(Filters.eq("author", "罗贯中"));
```

Filters里边还有其他的查询条件，都是见名知意，不赘述。

**查**

可以直接查询所有文档：

```java
FindIterable<Document> documents = c.find(); 
MongoCursor<Document> iterator = documents.iterator(); while (iterator.hasNext()) {   System.out.println(iterator.next()); } 
```

也可以按照条件查询：

```java
FindIterable<Document> documents = c.find(Filters.eq("author", "罗贯中")); MongoCursor<Document> iterator = documents.iterator(); while (iterator.hasNext()) {   System.out.println(iterator.next()); }
```

其他的方法基本都是见名知意，这里不再赘述。

**关闭连接**

```java
mongoClient.close();
```

**验证问题**

上面我们演示的获取一个集合是不需要登录MongoDB数据库的，如果需要登录，我们获取集合的方式改为下面这种：
```java
ServerAddress serverAddress = new ServerAddress("192.168.248.128", 27017); List<MongoCredential> credentialsList = new ArrayList<MongoCredential>(); 
MongoCredential mc = MongoCredential.createScramSha1Credential("readuser","sang","123".toCharArray()); credentialsList.add(mc); 
MongoClient client = new MongoClient(serverAddress,credentialsList); 
MongoDatabase sang = client.getDatabase("sang"); c = sang.getCollection("c1");
```

MongoCredential是一个凭证，第一个参数为用户名，第二个参数是要在哪个数据库中验证，第三个参数是密码的char数组，然后将登录地址封装成一个ServerAddress，最后将两个参数都传入MongoClient中实现登录功能。

**其他配置**

在连接数据库的时候也可以设置连接超时等信息，在MongoClientOptions中设置即可，设置方式如下：

```java
ServerAddress serverAddress = new ServerAddress("192.168.248.128", 27017);
List<MongoCredential> credentialsList = new ArrayList<MongoCredential>();
MongoCredential mc = MongoCredential.createScramSha1Credential("rwuser","sang","123".toCharArray());
credentialsList.add(mc);
MongoClientOptions options = MongoClientOptions.builder()
        //设置连接超时时间为10s
        .connectTimeout(1000*10)
        //设置最长等待时间为10s
        .maxWaitTime(1000*10)
        .build();
MongoClient client = new MongoClient(serverAddress,credentialsList,options);
MongoDatabase sang = client.getDatabase("sang");
c = sang.getCollection("c1");
```

参考：

https://segmentfault.com/a/1190000012705470

https://blog.csdn.net/hotdust/article/details/51315197