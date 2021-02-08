get请求参数中有list的情况：

```java
String url = "http://域名?list=...&list=..."
```

```java
//后台
@GetMapping(...)
public void test(List list){
    ...
}
```

这样就可以传递list参数了

get请求参数中有map的情况：

```java
Map<String, Object> hashMap = new HashMap<>();
hashMap.put("map","{\"a\":\"a\"}");
restTemplate.getForObject("http://域名?map={map}",Object.class,hashMap)
```

```java
//后台
@GetMapping(...)
public void test(Map map){
    ...
}
```

这样就可以传递map参数了(有个要注意的点参数map的key需要是String类型的，Object类型就不行)

参考链接：https://blog.csdn.net/weixin_42065545/article/details/90244454