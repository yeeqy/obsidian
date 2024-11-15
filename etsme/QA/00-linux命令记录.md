
1. 打印文件时带行号：`cat -n`
2. 查看两个文件区别可以直接用 `md5sum` 查看区别，不一致自己比较
3. 当出现bash：未找到命令的提示时，执行：`source /etc/profile`
4. **打印环境变量：`echo $PATH`（关于[[echo]]详细介绍）**
5. 直接在原地打印指定目录下的所有内容，不用cd到那个地方去看：`ls /opt/etsme`
6. 查找某个可执行文件的位置：`which xxx`，查找所有位置：`which -a xxx`（[[which]]）
7. sudo su 失败sudo: /usr/bin/sudo 必须属于用户 ID 0(的用户)并且设置 setuid 位：`su root`
8. 检查你的系统架构，然后下载与之匹配的 Go 版本。使用以下命令检查系统架构：`uname -m`
9. 适配aarch64的go包是wget https://mirrors.aliyun.com/golang/go1.20.linux-arm64.tar.gz
10. `go1.20.linux-amd64.tar.gz` 并不适用于 `aarch64`（ARM 64位）架构。`amd64` 是专为 **x86_64** 架构（如 Intel 和 AMD 处理器）设计的，而 **aarch64** 需要使用 `arm64` 版本的安装包。

11. 检查用户有没有在云端注册成功：`cat /userdata/sysmgmt/user.json`

### cmccstarry
---
1. 服务重启：
```
systemctl restart etsme-cmccstarry
```



### rk
---
1. 查看进程：`ps -A`
2. 快速验证rk的方式： 
```
systemctl status etsme-rkImageAnalyze.service
```


### usermgmt
---
```
systemctl status etsme-usermgmt.service
```

### niginx
---
1. nginx服务重启：
```
nginx -s reload
```

#### example
```
#!/bin/sh

dir=`dirname $(realpath $0)`
echo $dir
echo `realpath $0`
# ./nginx -t 
$dir/nginx -t

nginx -t
```