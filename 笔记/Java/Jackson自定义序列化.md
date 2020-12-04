# Jackson自定义序列化

使用@JsonSerialize 注解标注 需要自定义序列化的字段

```
@JsonSerialize(using = 自定义的序列化类.class)
```

```java
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.databind.JsonSerializer;
import com.fasterxml.jackson.databind.SerializerProvider;

import java.io.IOException;
import java.util.List;
import java.util.stream.Collectors;

public class ... extends JsonSerializer<List> {

    @Override
    public void serialize(List list, JsonGenerator jsonGenerator, SerializerProvider serializerProvider) throws IOException {
        jsonGenerator.writeObject(list.stream().distinct().collect(Collectors.toList()));
    }
}
```

自定义序列化类 继承 JsonSerializer类 重写 serialize方法，把自定义的序列化逻辑写进去