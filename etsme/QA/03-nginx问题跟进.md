---
tags:
  - 外部存储
cssclasses:
  - 繁星智算
  - TCN200
---
## 背景
---
> 存在bug：【TCN200繁星智算】外部存储部分文件无法预览

- 后端定位到为：
nginx 配置问题

- 网络定位：
版本上的 nginx 配置文件 etsme.conf 在打包时被外部文件覆盖了。
该项目所对应的 rootfs 未进行同步

---
## Notes

查看配置文件
```
cat /opt/etsme/etc/nginx/sites-available/etsme.conf
```
该配置文件下第60行开始不对

修改前：
```
			location ~* ^/v1/estor/(.*)$ {
    61          proxy_set_header Host $host;
    62          proxy_set_header X-Real-IP $remote_addr;
    63          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    64          proxy_pass http://127.0.0.1:18502/$1?$query_string;
    65      }

```

修改后：
```
    60      location ^~ /v1/estor/ {
    61          rewrite ^/v1/estor/(.*)$ /$1 break;
    62          proxy_set_header Host $host;
    63          proxy_set_header X-Real-IP $remote_addr;
    64          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    65          proxy_pass http://127.0.0.1:18502;
    66      }
```
然后重启
```
nginx -s reload
```

> 可以先用 `nginx -t` 看服务是否正常
> 如果未找到命令，就先`source /etc/pro` 或者`. /etc/profile`




### kimi解析

两段Nginx配置都用于处理以`/v1/estor/`开头的请求，并将这些请求代理到本地的`127.0.0.1:18502`服务。不过，它们在处理请求时有一些关键的区别：

1. **匹配模式**：
   - 第一段配置使用`^~`，这是一个非正则表达式的前缀匹配，这意味着如果Nginx找到了一个以`/v1/estor/`开头的location块，它将不会被后续的正则表达式location块覆盖。
   - 第二段配置使用`~*`，这是一个大小写不敏感的正则表达式匹配。这意味着如果存在其他正则表达式location块，Nginx将根据正则表达式的优先级来决定使用哪个location块。

2. **请求重写**：
   - 在第一段配置中，使用了`rewrite`指令来重写请求的URI，去掉了`/v1/estor/`前缀。
   - 在第二段配置中，没有使用`rewrite`指令，因此请求的URI不会被修改，`/v1/estor/`前缀会被保留。

3. **代理请求URI**：
   - 第一段配置中的`proxy_pass`指令直接将请求代理到`http://127.0.0.1:18502`，没有包含原始请求中的路径和查询字符串。
   - 第二段配置中的`proxy_pass`指令使用了`/$1?$query_string`，这意味着代理请求的URI将包含原始请求中`/v1/estor/`之后的部分（由`$1`表示），并且会保留原始请求的查询字符串（由`$query_string`表示）。

4. **转发行为**：
   - 第一段配置中，由于使用了`rewrite`指令，请求的URI被重写，然后整个请求（不包含`/v1/estor/`前缀）被代理到后端服务。
   - 第二段配置中，请求的URI不会被重写，而是将原始请求的路径和查询字符串一起代理到后端服务。

总结来说，第一段配置通过重写规则去除了请求URI中的`/v1/estor/`前缀，然后代理请求。而第二段配置保留了请求URI中的`/v1/estor/`前缀，并将其连同查询字符串一起代理到后端服务。这意味着后端服务接收到的请求路径和查询参数在两种配置下会有所不同。
