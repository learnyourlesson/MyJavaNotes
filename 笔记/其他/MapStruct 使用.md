# MapStruct 使用

MapStruct 是对象映射的工具，还有其他一些其他的工具，我看了[5种常见Bean映射工具的性能比对](https://www.shuzhiduo.com/A/gAJGplk15Z/)这个后选择了 MapStruct,它有个问题，使用注解时@Mapper和@Mapping 如果你使用的mabatis的话，要注意，我使用那个项目是mongodb的就没有这个问题。

## 使用

```xml
<dependency>
    <groupId>org.mapstruct</groupId>
    <!-- jdk8以下就使用mapstruct -->
    <artifactId>mapstruct-jdk8</artifactId>
    <version>1.2.0.Final</version>
</dependency>
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-processor</artifactId>
    <version>1.2.0.CR1</version>
    <scope>provided</scope>
</dependency>
```

**注意：**swagger2会有包冲突，排除一下

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.7.0</version>
    <exclusions>
        <exclusion>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```



```java
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.factory.Mappers;

@Mapper(componentModel = "spring")
public interface ViewMapper{
    //    ViewMapper INSTANCE = Mappers.getMapper(ViewMapper.class);

    @Mapping(source ="project.projectId",target = "projectId")
    public View from(BaseProject baseProject, Project project);
}
```

`@Mapper(componentModel = "spring")`使用了这个注解就可以使用自动注入来使用了对应的转换类，或者使用单例模式来使用

```java
@Autowired //自动注入
private ViewMapper ViewMapper;

View view = viewMapper.from(baseProject, project);
```

转换规则：

1.如果时属性名相同的就会自动转换，2转1时，如果有同名属性会有一些奇怪的表现，（我使用时是所有属性为空），需要指定源属性（跟属性不同一样）

2.`@Mapping(source = "totalCount", target = "count")`:`source`是源属性，`target`目标属性，如果是对象中属性可以用`.`来指定`@Mapping(source ="project.projectId",target = "projectId")`,还可以数据直接塞进去

```java
// 例子
@Mapping(source ="list",target = "list")
public ProjectView from(BaseProject baseProject, List list);

```

多个`@Mapping`可以使用`@Mappings`包起来

```
@Mappings({
        @Mapping(source = "person.description", target = "description"),
        @Mapping(source = "address.houseNo", target = "houseNumber")
    })
```



## 更新 Bean 对象(未尝试过)

有时候， 我们不是想返回一个新的 Bean 对象， 而是希望更新传入对象的一些属性。这个在实际的时候也会经常使用到。

在 `AddressMapper` 类中， 新增如下方法

```java
    /**
     * Person->DeliveryAddress, 缺失地址信息
     * @param person
     * @return
     */
    DeliveryAddress person2deliveryAddress(Person person);

    /**
     * 更新， 使用 Address 来补全 DeliveryAddress 信息。 注意注解 @MappingTarget
     * @param address
     * @param deliveryAddress
     */
    void updateDeliveryAddressFromAddress(Address address,
                                          @MappingTarget DeliveryAddress deliveryAddress);
```

注解 `@MappingTarget`后面跟的对象会被更新。 以上的代码可以通过以下的测试。

```java
@Test
public void updateDeliveryAddressFromAddress() {
    Person person = new Person();
    person.setFirstName("first");
    person.setDescription("perSonDescription");
    person.setHeight(183);
    person.setLastName("homejim");

    DeliveryAddress deliveryAddress = AddressMapper.INSTANCE.person2deliveryAddress(person);
    assertEquals(deliveryAddress.getFirstName(), person.getFirstName());
    assertNull(deliveryAddress.getStreet());

    Address address = new Address();
    address.setDescription("addressDescription");
    address.setHouseNo(29);
    address.setStreet("street");
    address.setZipCode(344);

    AddressMapper.INSTANCE.updateDeliveryAddressFromAddress(address, deliveryAddress);
    assertNotNull(deliveryAddress.getStreet());
}
```



参考链接：

https://www.shuzhiduo.com/A/gAJGplk15Z/

https://www.cnblogs.com/homejim/p/11313128.html

https://mapstruct.org/

https://yunlongn.github.io/2019/05/29/%E5%AF%B9%E8%B1%A1%E6%8B%B7%E8%B4%9D-%E4%BC%98%E9%9B%85%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88-Mapstruct/