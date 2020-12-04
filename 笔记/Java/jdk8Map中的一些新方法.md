# jdk8Map中的一些新方法

### computeIfAbsent（）

如果 map中不存在对应的key,就把新key，value存进map，新value为null就什么都不做

```java
// 方法定义
default V computeIfAbsent(K key, Function<? super K,?extends V>mappingFunction) { ... }

// java8之前。从map中根据key获取value操作可能会有下面的操作

Object key = map.get("key"); 
if (key == null) {    
	key = new Object();    
	map.put("key", key); 
} 

// java8之后。上面的操作可以简化为一行，若key对应的value为空，会将第二个参数的返回值存入并返回

Object key2 = map.computeIfAbsent("key", k -> new Object());




HashMap<String, String> map = new HashMap<>();
String a1 = map.put("a", "1");
System.out.println(a1);
System.out.println("**********"+map);
String s = map.computeIfAbsent("a", key -> "aa");
System.out.println(s);
System.out.println("**********"+map);
String s1 = map.computeIfAbsent("b", key->"bb");
System.out.println(s1);
System.out.println("**********"+map);
String s2 = map.computeIfAbsent("b", key->null);
System.out.println(s2);
System.out.println("**********"+map);
String s3 = map.computeIfAbsent("c", key->null);
System.out.println(s3);
System.out.println("**********"+map);

结果：
null
**********{a=1}
1
**********{a=1}
bb
**********{a=1, b=bb}
bb
**********{a=1, b=bb}
null
**********{a=1, b=bb}
```

### V putIfAbsent （V key, V value）

如果 key 不存在或相关联的值为 null, 则设置新的 key/value 值,原map中key不存在或者key对应的value为null 返回 null,否则返回旧值，

```java
HashMap<String, String> map = new HashMap<>();
String s = map.putIfAbsent("11", null);
String s1 = map.putIfAbsent("11", "1");
String s2 = map.putIfAbsent("11", "2");
System.out.println(s);
System.out.println(s1);
System.out.println(s2);
System.out.println(map);


结果：
null
null
1
{11=1}
```

### computeIfPresent （）

原来的map有 key 对应的不为 null 的value时，根据key,value 修改 value（新value不为null，否则删除key）

```java
HashMap<String, String> map = new HashMap<>();
String s = map.putIfAbsent("a", "1");
System.out.println(s);
System.out.println("**********"+map.toString());
String s1 = map.computeIfPresent("a", (key,value)->"aa");
System.out.println(s1);
System.out.println("**********"+map.toString());
String s2 = map.computeIfPresent("a", (key,value)->null);
System.out.println(s2);
System.out.println("**********"+map.toString());
String s3 = map.computeIfPresent("a", (key,value)->"bb");
System.out.println(s3);
System.out.println("**********"+map);

结果：
null
**********{a=1}
aa
**********{a=aa}
null
**********{}
null
**********{}
```



参考：

https://blog.csdn.net/weixin_38229356/article/details/81129320

https://blog.csdn.net/qq_41701956/article/details/83620297