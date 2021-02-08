## 利用@ControllerAdvice与@ExceptionHandler注解实现异常的全局处理

@ControllerAdvice是在类上声明的注解，与其他注解组合,实现不同的定制功能,其用法主要有三点：

- @ExceptionHandler注解标注的方法：用于捕获Controller中抛出的不同类型的异常，从而达到异常全局处理的目的；
- @InitBinder注解标注的方法：用于请求中注册自定义参数的解析，从而达到自定义请求参数格式的目的；
- @ModelAttribute注解标注的方法：表示此方法会在执行目标Controller方法之前执行 



### 在项目中实现异常全局处理的过程如下: 

- 自定义一个全局异常处理类,该类中并使用@ControllerAdvice修饰
- 使用@ExceptionHandler注解捕获指定或自定义的异常；
- 使用@ControllerAdvice集成@ExceptionHandler的方法到一个类中（类上要有@Controller/@RestController）；
- 必须定义一个通用的异常捕获方法，便于捕获未定义的异常信息；
- 自定一个异常类统一处理类，捕获针对项目或业务的异常;
- 异常的对象信息补充到统一结果枚举中；

```java
/**
* 异常处理controller
*/
@RestController
@ControllerAdvice
public class ExceptionHandleController {
    @ExceptionHandler(value= ErrorMessageException.class)
    public Response errorMessageResponse(ErrorMessageException ex) {
        return Response.failureInstance(ex.getMessage(), ex.getErrorCode());
    }
}
```

```
/**
 * Created on  2021/1/7
 * 异常同一处理类
 */
public class ErrorMessageException extends RuntimeException {
    private static final Logger logger = LoggerFactory.getLogger(ErrorMessageException.class);

    private int errorCode = 0;

    public ErrorMessageException(String errorMessage, Throwable cause) {
        super(errorMessage, cause);
        logger.error(errorMessage);
        if(cause != null) {
            logger.error(cause.getMessage());
        }
    }

    public ErrorMessageException(String errorMessage) {
        super(errorMessage);
        logger.error(errorMessage);
    }

    public ErrorMessageException(String errorMessage, int errorCode) {
        this(errorMessage);
        this.errorCode = errorCode;
    }

    public ErrorMessageException(String errorMessage, int errorCode, Throwable cause) {
        this(errorMessage, cause);
        this.errorCode = errorCode;
    }

    public int getErrorCode() {
        return errorCode;
    }

    public void setErrorCode(int errorCode) {
        this.errorCode = errorCode;
    }
}
```

```java
/**
 * 定义统一的http返回值
 */
@JsonIgnoreProperties(ignoreUnknown = true)
public class Response<T> {
    @JsonProperty("success")
    private boolean success;

    @JsonProperty("data")
    private T data;

    @JsonProperty("message")
    private String message;

    @JsonProperty("errorCode")
    private int errorCode;

    public Response(
            @JsonProperty("success") boolean success,
            @JsonProperty("data") T data,
            @JsonProperty("message") String message,
            @JsonProperty("errorCode") int errorCode) {
        this.success = success;
        this.data = data;
        this.message = message;
        this.errorCode = errorCode;
    }

    public static <T> Response<T> successInstance(T data) {
        return new Response<>(true, data, null, 0);
    }

    public static Response failureInstance(String message, int errorCode) {
        return new Response<>(false, null, message, errorCode);
    }

    public static Response failureInstance(String message) {
        return new Response<>(false, null, message, 0);
    }

    public boolean getSuccess() {
        return success;
    }

    public void setSuccess(boolean success) {
        this.success = success;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public int getErrorCode() {
        return errorCode;
    }

    public void setErrorCode(int errorCode) {
        this.errorCode = errorCode;
    }

    @Override
    public String toString() {
        return "Response{" +
                "success=" + success +
                ", data=" + data +
                ", message='" + message + '\'' +
                ", errorCode=" + errorCode +
                '}';
    }
}
```

