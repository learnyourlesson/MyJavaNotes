原来的克隆：

```git
git clone https://remote
```

修改为

```git
git clone https://[username]:[password]@/remote
```

但是有时会出现用户名或密码中有 @ 这样的特殊符号时，而导致url不能被正确解析，

所以需要对特殊符号进行url编码

```java
String c = URLEncoder.encode("@","utf-8");
System.out.println(c);
console -> %40
```

这个网站也可以（或者搜索 urlEncode 在线）https://tool.chinaz.com/tools/urlencode.aspx



参考：https://www.bbsmax.com/A/QV5ZeanVJy/