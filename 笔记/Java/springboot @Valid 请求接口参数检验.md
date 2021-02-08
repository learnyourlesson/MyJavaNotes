### 嵌套注解 @Valid（javax.validation.Valid）@Validated

1. `@Valid`是JSR-303标准规定的；`@Validated`是Spring校验框架提供的
2. `@Valid`可以在方法/成员变量/构造函数/方法参数上使用；`@Validated`可以在类/方法/方法参数上使用；区别在于成员变量上是否可以使用
3. 由于`@Valid`可以在成员变量上使用，因此可以嵌套校验
4. `@Valid`会把校验不通过的信息交给`BindingResult`中，因此在`controller`的方法中，`@Valid`和`BindingResult`要同时存在
5. `@Validated`可以在类上使用，可以配合`MethodValidationPostProcessor`实现校验基本数据包装类型和`String`类型等单独的对象

### 空和非空检查: @Null、@NotNull、@NotBlank、@NotEmpty

|   注解    |              支持Java类型              |                             备注                             |
| :-------: | :------------------------------------: | :----------------------------------------------------------: |
|   @Null   |                 Object                 |                       验证元素值为null                       |
| @NotNull  |                 Object                 |                    验证元素值不能为 null                     |
| @NotBlank |              CharSequence              |         验证元素值不为null且移除两边空格后长度大于0          |
| @NotEmpty | CharSequence,Collection,Map and Arrays | 验证元素值不为null且不为空（字符串长度不为0、集合大小不为0） |

### Boolean值检查: @AssertTrue、@AssertFalse

|     注解     |   支持Java类型   |               备注               |
| :----------: | :--------------: | :------------------------------: |
| @AssertTrue  | Boolean, boolean | 验证元素值必须为true，否则抛异常 |
| @AssertFalse | Boolean, boolean |      验证元素值必须为flase       |

### 长度检查: @Size、@Length

|  注解   |               支持Java类型                |            备注            |
| :-----: | :---------------------------------------: | :------------------------: |
|  @Size  | String,Collection,Map,arrays,CharSequence | 验证元素个数包含在一个区间 |
| @Length |               CharSequence                |  验证元素值包含在一个区间  |

### 日期检查: @Future、@FutureOrPresent、@Past、@PastOrPresent

|       注解       |            支持Java类型            |               备注               |
| :--------------: | :--------------------------------: | :------------------------------: |
|     @Future      | java.util.Date, java.util.Calendar |      验证日期为当前时间之后      |
| @FutureOrPresent | java.util.Date, java.util.Calendar | 验证日期为当前时间或之后一个时间 |
|      @Past       | java.util.Date, java.util.Calendar |      验证日期为当前时间之前      |
|  @PastOrPresent  | java.util.Date, java.util.Calendar |     验证日期为当前时间或之前     |

### 其它检查: @Email、@CreditCardNumber、@URL、@Pattern、@ScriptAssert、@UniqueElements

|       注解        | 支持Java类型 |                 备注                  |
| :---------------: | :----------: | :-----------------------------------: |
|      @Email       | CharSequence |        验证日期为当前时间之后         |
| @CreditCardNumber | CharSequence |   验证日期为当前时间或之后一个时间    |
|       @URL        | CharSequence |        验证日期为当前时间之前         |
|     @Pattern      | CharSequence |       验证日期为当前时间或之前        |
|      @Valid       |    Object    |   验证关联对象元素进行递归校验检查    |
|  @UniqueElements  |  Collection  | 校验集合中的元素必须保持唯一 否则异常 |

### 数值检查: @Min、@Max、@Range、@DecimalMin、@DecimalMax、@Digits

|                注解                |                     支持Java类型                      |                   备注                   |
| :--------------------------------: | :---------------------------------------------------: | :--------------------------------------: |
|                @Min                | BigDecimal, BigInteger, byte, short,int, long,Number. |        检验当前数值大于等于指定值        |
|                @Max                |                     CharSequence                      |        检验当前数值小于等于指定值        |
|               @Range               |                     CharSequence                      |        验证数值为指定值区间范围内        |
|            @DecimalMin             |                     CharSequence                      |        验证数值是否大于等于指定值        |
|            @DecimalMax             |                        Object                         |        验证数值是否小于等于指定值        |
| @Digits(integer = 3, fraction = 2) |                                                       | 验证注解的元素值的整数位数和小数位数上限 |




参考链接：https://www.jianshu.com/p/4dfd387cdc19
