```java

private static final MediaType BODY_ENCODE = MediaType.parse("application/json; charset=utf-8");//上传数据的格式
String url = "Url";
RequestBody requestBody = RequestBody.create(BODY_ENCODE, file);

MultipartBody multipartBody;

multipartBody = new MultipartBody.Builder()
        .setType(MultipartBody.FORM)//设置提交的是form表单
        .addFormDataPart()//添加请求头
        .addFormDataPart("file", file.getName(), requestBody)//添加请求body
        .build();//创建对象
        
Request request;
request = new Request.Builder()
        .addHeader("Accept", "application/json")//添加请求头
        .url(new URL(url))
        .method(method, body)//添加请求方式和body
        .build();
Response response = client.newCall(request).execute();//获得响应
return response.body() != null ? response.body().string() : null;//获得响应体


```

