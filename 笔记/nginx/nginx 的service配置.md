要检测现有的修改过的Nginx配置是否有错误，不是单单检测那个修改过的扩展的`.conf`文件。而是不管任何时候，始终都是去检测主文件`/etc/nginx/nginx.conf`，只有这样，才能顺利的在对应的模块加载扩展的`.conf`文件。

这样一来保证了配置的前后语境的正确性，二来，这样才是真正的检测（完全和实际运行情况相符）

所以正确的检测修改的Nginx的语法是否错误的命令应该是：`sudo nginx -t -c /etc/nginx/nginx.conf`或者``sudo nginx -t`(默认是`/etc/nginx/nginx.conf`)



比如：http://www.xxx.com/hello/test/ ，那么$uri/就是 /hello/test/

完整的解释就是：try_files 去尝试到网站目录读取用户访问的文件，如果第一个变量存在，就直接返回；
不存在继续读取第二个变量，如果存在，直接返回；不存在直接跳转到第三个参数上。

比如用户访问这个网地址：http://www.xxx.com/test.html
try_files首先会判断出他是文件，还是一个目录，结果发现他是文件，与第一个参数 $uri变量匹配。
然后去到网站目录下去查找test.html文件是否存在，如果存在直接读取返回。如果不存在直接跳转到第三个参数，而第三个参数是一个location,而这个location里面配置的就是rewrite规则。

## try_files也可以最后是重定向到一个地址：try_files $uri $uri/ /index.php;

```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

当用户请求 http://localhost/example 时，这里的 $uri 就是 /example。
try_files 会到硬盘里尝试找这个文件。如果存在名为 /$root/example（其中 $root 是项目代码安装目录）的文件，就直接把这个文件的内容发送给用户。
显然，目录中没有叫 example 的文件。然后就看 $uri/，增加了一个 /，也就是看有没有名为 /$root/example/ 的目录。
又找不到，就会 fall back 到 try_files 的最后一个选项 /index.php，发起一个内部 “子请求”，也就是相当于 nginx 发起一个 HTTP 请求到 http://localhost/index.php。

## rewrite 语法格式：

```
rewrite     [regex]          [replacement]      [flag];
            url正则表达式     替换成真实url       标记（last,break）
            
location @router {
    rewrite ^.*$ /index.html last;
    #     匹配所有	/index.html last;
}
1234567
```

flag 支持以下 4 个选项：

last：停止处理当前的 ngx_http_rewrite_module 指令集，并开始对匹配更改后的 URI 的新 location 进行搜索（再从 server 走一遍匹配流程）。此时对于当前 server 或 location 上下文，不再处理 ngx_http_rewrite_module 重写模块的指令。
break：停止处理当前的 ngx_http_rewrite_module 指令集
redirect：返回包含 302 代码的临时重定向，在替换字符串不以“http://”，“https://”或“$scheme”开头时使用
permanent：返回包含 301 代码的永久重定向。
last 和 break 的区别及共同处：

last 重写 url 后，会再从 server 走一遍匹配流程，而 break 终止重写后的匹配
break 和 last 都能阻止后面的 rewrite 指令再次执行



在nginx中配置proxy_pass代理转发时，如果在proxy_pass后面的url加/，表示绝对根路径；如果没有/，表示相对路径，把匹配的路径部分也给代理走。

假设下面四种情况分别用 http://192.168.1.1/proxy/test.html 进行访问。

第一种：
 location /proxy/ {
 proxy_pass http://127.0.0.1/;
 }
 代理到URL：http://127.0.0.1/test.html

第二种（相对于第一种，最后少一个 / ）
 location /proxy/ {
 proxy_pass http://127.0.0.1;
 }
 代理到URL：http://127.0.0.1/proxy/test.html

第三种：
 location /proxy/ {
 proxy_pass http://127.0.0.1/aaa/;
 }
 代理到URL：http://127.0.0.1/aaa/test.html

第四种（相对于第三种，最后少一个 / ）
 location /proxy/ {
 proxy_pass http://127.0.0.1/aaa;
 }
 代理到URL：http://127.0.0.1/aaatest.html

## nginx跨域配置

流程如下

- 首先查看http头部有无origin字段；
- 如果没有，或者不允许，直接当成普通请求处理，结束；
- 如果有并且是允许的，那么再看是否是preflight(method=OPTIONS)；
- 如果是preflight，就返回Allow-Headers、Allow-Methods等，内容为空；
- 如果不是preflight，就返回Allow-Origin、Allow-Credentials等，并返回正常内容



```conf

set $cors '';
# 对于 域名的区分大小写不匹配（这样配置代表的是都添加跨域请求头（有点问题，但是我们是内网项目就无所谓了））
#~ 为区分大小写匹配
#~* 为不区分大小写匹配
#!~和!~*分别为区分大小写不匹配及不区分大小写不匹配
if ($http_origin !~ '^http://www.baidu.com') {
set $cors 'true';
}
# http origin 为-的请求 请求报文
#GET /resources/public-data/ HTTP/1.1
#Host: bar.other
#User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) #Gecko/20081130 Minefield/3.1b3pre
#Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
#Accept-Language: en-us,en;q=0.5
#Accept-Encoding: gzip,deflate
#Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
#Connection: keep-alive
#Referer: http://foo.example/examples/access-control/simpleXSInvocation.html
#Origin: http://foo.example

if ($http_origin ~ '^-$') {
	set $cors '';
}

# 非简单请求的试探请求，对于非简单请求，CORS 机制跨域会首先进行 preflight（一个 OPTIONS 请求）
if ($request_method = 'OPTIONS') {
add_header 'Access-Control-Allow-Origin' "$http_origin" always;
add_header 'Access-Control-Allow-Credentials' 'true' always;
add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS, PATCH' always;
add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With, Authorization' always;
# Tell client that this pre-flight info is valid for 20 days
add_header 'Access-Control-Max-Age' 1728000 always;
add_header 'Content-Type' 'text/plain charset=UTF-8' always;
add_header 'Content-Length' 0 always;
return 204;
}
               
# 其他请求
if ($cors = 'true') {
add_header 'Access-Control-Allow-Origin' "$http_origin" always;
add_header 'Access-Control-Allow-Credentials' 'true' always;
add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS, PATCH' always;
add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With, Authorization' always;
}

# 携带上级代理的信息或用户真实ip（无多次代理）
proxy_set_header X-Real-IP $remote_addr;
# 携带所有代理以及用户真实ip
proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Host $http_host;
proxy_pass http://www.baidu.com/;

```



参考链接：https://blog.csdn.net/qq_23035335/article/details/102772037

​					https://www.jianshu.com/p/b010c9302cd0

https://blog.csdn.net/weiyuefei/article/details/78687545

https://www.cnblogs.com/kevingrace/p/8269955.html

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS