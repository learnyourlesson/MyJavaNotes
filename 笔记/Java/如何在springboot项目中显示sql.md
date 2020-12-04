1. 在配置文件中写入（yml的写法，properties变一下就行）

```properties
logging:
  level:
    org.springframework.data.mongodb.core: DEBUG
```

使用SLF4J和Logback打印日志

2. 或者使用断点查看sql对象