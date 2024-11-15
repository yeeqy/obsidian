
## 方法
---
win+R
cmd
ping github.com，获取ip地址
打开 C:\Windows\System32\drivers\etc
用记事本打开hosts文件
复制：ip github.com

保存文件，退出，大功告成！  
**PS：这时候应该已经成功了，打开github应该已经变得飞快了，只有当你还是打不开github或速度没有任何提升，才需要继续往下看**  


2、再次打开cmd，输入如下命令刷新DNS缓存：

```bash
ipconfig/flushdns
```

## 解决方案二

打开[https://sites.ipaddress.com/github.com/](https://sites.ipaddress.com/github.com/)找到DNS Resource Records，复制github的ip地址，先保存起来