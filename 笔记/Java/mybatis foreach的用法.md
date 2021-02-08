注解版

```java
@Select({"<script>",
            "SELECT * FROM table AS t WHERE ",
            "(`t`.`id` ",
            "<foreach collection='emailNameList' item='toolName' open='(' separator=',' close=')'>",
            "#{toolName}",
            "</foreach>)",
 
            "</script>"})
publice List<Map<String,Object>> test(@Param("emailNameList")List emailNameList );
```

注意点，换行使用`,` collection 是要遍历的元素，item 就是循环中的临时变量， open 开始符号， separator 分隔符， close 结束符号 ，