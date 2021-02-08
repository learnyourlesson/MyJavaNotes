# 使用@Query使用mongodb

```java
@Query("{'列名':?0（一号位参数）,'status':{'$nin（不属于）':?1}}")
```

### 在基于json的查询中使用SpEL表达式

查询串和field返回定义可以使用SpEL表达式 在运行时进行动态创建 。
 表达式通过包含所有参数的数组公开方法参数。 以下查询使用[0]声明lastname的谓词值（相当于?0参数绑定）：



```kotlin
public interface PersonRepository extends MongoRepository<Person, String>

  @Query("{'lastname': ?#{[0]} }")
  List<Person> findByQueryWithExpression(String param0);
}
```

当传入参数为对象时， 实例：



```csharp
    @Query(value="{'name': ?#{ [0].name }}")
    public Page<RcControllJournalDo> querylikepages(RcControllJournalDo mdo, Pageable pageable);
```

上例等价于 where name = mdo.name .

更复杂的实例：



```bash
    /**
     * 当mdo.name为空时， 查询条件为  { "name" : { "$exists" : true } } ，即查询所有name列存在的记录（包括值为null的记录，但是对于没有name列的查询不到） ；
     * 当mdo.name不空时，查询条件为   { "name" : [0].name }
     */
    @Query(value=" { 'name': ?#{ ([0].name == null) or ([0].name.length() == 0)  ? '{$exists:true}' : [0].name } } ")
    public Page<RcControllJournalDo> querylikepages2(RcControllJournalDo mdo, Pageable pageable);
```

`#{ ([0].name == null) or ([0].name.length() == 0) ? '{$exists:true}' : [0].name }` 为SpEL表达式  （三目表达式）。

模糊查询例子：



```kotlin
    /**
     * 使用正则表达式模糊查询 
     */
    @Query(value=" { 'idno': ?#{ ([0].name == null) or ([0].name.length() == 0)  ? {$exists:true} : {$regex: [0].name } } } ")
    public Page<RcControllJournalDo> querylikepages21(RcControllJournalDo mdo, Pageable pageable);
```

mongodb的正则表达式查询语法为：



```css
>db.posts.find({post_text:{$regex:"runoob"}})
>db.posts.find({post_text:{$regex:"runoob",$options:"$i"}}) 
```

例子：
 根据前端上送的查询条件模糊匹配name 和idno  ， 当有值时查询之，无则查询所有：



```bash
    /**
     * 模糊查询name 和 idno  <br>
     * 1. mongodb or语法  ：{ $or :[{}, {},...] }  例子： {$or:[{"by":"aaa"} , {"title": "bbb"}]}  ， 即 where by=aaa or title=bbb <BR>
     * 2. { $or :[{'name' : ?#{}}, {'idno' : ?#{}}] }  <br>
     * 
     */
    @Query(value=" { $or :[{'name' : ?#{ ([0].name == null) or ([0].name.length() == 0)  ? '{$exists:true}' :  {$regex:[0].name} }},"
            + " {'idno' : ?#{ ([0].idno == null) or ([0].idno.length() == 0)  ? '{$exists:true}' : {$regex: [0].idno} }}] } ")
    public Page<RcControllJournalDo> querylikepages3(RcControllJournalDo mdo, Pageable pageable);
```

输入参数：
 mdo.setName("宋");
 mdo.setIdno("112");
 打印的日志为：



```bash
find using query: { "$or" : [{ "name" : { "$regex" : "宋" } }, { "idno" : { "$regex" : "112" 
```





参考链接：https://www.jianshu.com/p/24a44c4c7651
